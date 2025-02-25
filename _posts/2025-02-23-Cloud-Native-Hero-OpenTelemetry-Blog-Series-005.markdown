---
layout: post
title:  "Cloud Native Hero Newsletter -  How to Set Up OpenTelemetry in Your Application"
date:   2025-02-23 00:00:00 +0530
categories:  observability opentelemetry 
author: coolsvap
permalink: /cloud-native-hero-opentelemetry-blog-series-005-setup-opentelemetry
---
# How to Set Up OpenTelemetry in Your Application to Send Data to the Otel Collector on Kubernetes

## Introduction
Deploying applications in **Kubernetes** (K8s) environments adds complexity to observability due to the **distributed nature** of microservices and dynamic infrastructure. **OpenTelemetry (OTel)**, combined with the **OpenTelemetry Collector** deployed in Kubernetes, provides a powerful and flexible observability pipeline for collecting, processing, and exporting **traces, metrics, and logs**.

This guide walks you through setting up OpenTelemetry in your application to send telemetry data to an **OpenTelemetry Collector running in Kubernetes**.

---

## 🚀 Prerequisites
Before you begin, ensure you have:
- A running **Kubernetes cluster** (e.g., Minikube, EKS, GKE, or AKS).
- **kubectl** configured to interact with your cluster.
- **Docker** (for building application images, if needed).
- Basic knowledge of Kubernetes resources (Deployments, Services, ConfigMaps).
- Helm (optional, for easier deployment of the OpenTelemetry Collector).

---

## 🛠️ Step 1: Deploy the OpenTelemetry Collector in Kubernetes
The **OpenTelemetry Collector** acts as an agent or gateway to receive telemetry data and export it to backends like **Jaeger, Prometheus, or Datadog**.

### ✅ Option A: Deploy Using Helm *(Recommended)*
1. Add the OpenTelemetry Helm repository:
```bash
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update
```

2. Install the Collector with a basic configuration:
```bash
helm install otel-collector open-telemetry/opentelemetry-collector -n observability --create-namespace
```

3. Verify the deployment:
```bash
kubectl get pods -n observability
```

### ✅ Option B: Deploy Using Kubernetes Manifests
Create a `otel-collector-config.yaml` for the Collector configuration:
```yaml
receivers:
  otlp:
    protocols:
      grpc:
      http:

exporters:
  logging:
    loglevel: debug
  jaeger:
    endpoint: "jaeger-collector:14250"
    tls:
      insecure: true

service:
  pipelines:
    traces:
      receivers: [otlp]
      exporters: [logging, jaeger]
```

Deploy the Collector:
```bash
kubectl apply -f otel-collector-config.yaml
```

---

## ⚙️ Step 2: Expose the OpenTelemetry Collector
Create a Kubernetes Service to expose the Collector for your application to send data.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: otel-collector
  namespace: observability
spec:
  ports:
    - name: grpc
      port: 4317
      targetPort: 4317
    - name: http
      port: 4318
      targetPort: 4318
  selector:
    app.kubernetes.io/name: opentelemetry-collector
```

Apply the service:
```bash
kubectl apply -f otel-collector-service.yaml
```

---

## 📥 Step 3: Instrument Your Application to Send Data
Your application needs to send telemetry data to the exposed **otel-collector** service using the **OTLP protocol**.

### ✅ Example Configuration for Different Languages
#### Python
```python
from opentelemetry import trace
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

otlp_exporter = OTLPSpanExporter(endpoint="otel-collector.observability.svc.cluster.local:4317")
trace.get_tracer_provider().add_span_processor(BatchSpanProcessor(otlp_exporter))

with tracer.start_as_current_span("example-span"):
    print("Sending trace to OpenTelemetry Collector in Kubernetes")
```

#### Node.js
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-grpc');
const { BatchSpanProcessor } = require('@opentelemetry/sdk-trace-base');

const provider = new NodeTracerProvider();
const exporter = new OTLPTraceExporter({ url: 'grpc://otel-collector.observability.svc.cluster.local:4317' });

provider.addSpanProcessor(new BatchSpanProcessor(exporter));
provider.register();

const tracer = provider.getTracer('example-service');
const span = tracer.startSpan('example-span');
console.log('Trace sent to Collector');
span.end();
```

#### Java
```java
import io.opentelemetry.api.trace.Span;
import io.opentelemetry.api.trace.Tracer;
import io.opentelemetry.sdk.trace.SdkTracerProvider;
import io.opentelemetry.exporter.otlp.trace.OtlpGrpcSpanExporter;
import io.opentelemetry.sdk.trace.export.BatchSpanProcessor;

SdkTracerProvider tracerProvider = SdkTracerProvider.builder()
    .addSpanProcessor(BatchSpanProcessor.builder(OtlpGrpcSpanExporter.builder()
        .setEndpoint("otel-collector.observability.svc.cluster.local:4317")
        .build())
    .build())
    .build();

Tracer tracer = tracerProvider.get("example-service");
Span span = tracer.spanBuilder("example-span").startSpan();
System.out.println("Sending trace to OpenTelemetry Collector");
span.end();
```

---

## 🖥️ Step 4: Visualize Data Using Jaeger and Prometheus
Deploy backends like **Jaeger** for tracing and **Prometheus** for metrics.

### ✅ Deploy Jaeger
```bash
kubectl create namespace tracing
kubectl apply -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/examples/simplest.yaml
```
Access Jaeger UI:
```bash
kubectl port-forward svc/jaeger-query 16686:16686 -n tracing
```
Visit: [http://localhost:16686](http://localhost:16686)

### ✅ Deploy Prometheus (Optional)
```bash
helm install prometheus prometheus-community/prometheus -n monitoring --create-namespace
```
Access Prometheus UI:
```bash
kubectl port-forward svc/prometheus-server 9090 -n monitoring
```
Visit: [http://localhost:9090](http://localhost:9090)

---

## 🔍 Step 5: Verify Data Flow
Once your application and the Collector are running:
- **Traces:** Check Jaeger to view spans and trace timelines.
- **Metrics:** Query Prometheus to see collected metrics.
- **Logs:** Use the Collector’s logging exporter for initial validation.

✅ **Tip:** Use Grafana to create dashboards combining metrics and traces for comprehensive observability.

---

## Conclusion
Setting up **OpenTelemetry** to send data from your application to the **OpenTelemetry Collector in Kubernetes** provides scalable and flexible observability. By following these steps, you gain real-time insights into your application’s performance, enabling faster debugging, improved reliability, and better end-user experiences.



## Complete OpenTelemetry Blog Series
- [OpenTelemetry Series #1 - A Beginner’s Guide](https://cloudnativehero.github.io/cloud-native-hero-opentelemetry-blog-series-001-beginners-guide)
- [OpenTelemetry Series #2 - OpenTelemetry vs. Traditional Monitoring: What’s the Difference?](https://cloudnativehero.github.io/cloud-native-hero-opentelemetry-blog-series-002-difference-traditional-monitoring)
- [OpenTelemetry Series #3 - The Three Pillars of Observability in OpenTelemetry](https://cloudnativehero.github.io/cloud-native-hero-opentelemetry-blog-series-003-three-pillars-of-opentelemetry)
- [OpenTelemetry Series #4 - Why Developers Should Care About OpenTelemetry](https://cloudnativehero.github.io/cloud-native-hero-opentelemetry-blog-series-004-why-opentelemetry)
- [OpenTelemetry Series #5 - How to Set Up OpenTelemetry in Your Application](https://cloudnativehero.github.io/cloud-native-hero-opentelemetry-blog-series-005-setup-opentelemetry)

---
Subscribe to the [Cloud Native Hero! Newsletter](https://www.linkedin.com/newsletters/6940180331832446978/) for regular updates.

Join the [Observability India LinkedIn Group] (https://www.linkedin.com/groups/9899111/)

---
[**LinkedIn**](https://www.linkedin.com/company/cloudnativehero/) | [**Twitter**](https://twitter.com/cloudnativehero) | [**GitHub**](https://github.com/cloudnativehero) | [**Blog**](https://cloudnativehero.github.io/)
