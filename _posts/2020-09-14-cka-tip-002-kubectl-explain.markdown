---
layout: post
title:  "CKA Tips - 002 Using kubectl explain"
date:   2020-09-14 17:50:01 +0530
categories: Kubernetes CKA Tips
author: coolsvap
permalink: /cka-tip-002-kubectl-explain
---

What would you do when you don't know what is the structure of any [Kubernetes][2] resource, what is the specification, types of attributes, etc, most of us probably reach for Google. But there’s a better way! `kubectl explain` is used to discover static information about Kubernetes Resources. [kubectl][1] `explain` command as it seems a good starting point to understand and know the static information for. In this post lets see how `kubectl explain` works

Here it is

```
$ kubectl explain pod

KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion   <string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

   kind <string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

   metadata     <Object>
     Standard object's metadata. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

   spec <Object>
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

   status       <Object>
     Most recently observed status of the pod. This data may not be up to date.
     Populated by the system. Read-only. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

```
Behind the scenes, kubectl just made an API request to my Kubernetes cluster, grabbed the current Swagger documentation of the API version running in the cluster, and output the documentation and object types.

what if I want to see all the fields in a Pod?

<details>
<summary>
`kubectl explain pod --recursive`
</summary>

```
$ kubectl explain pod --recursive

KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion   <string>
   kind <string>
   metadata     <Object>
      annotations       <map[string]string>
      clusterName       <string>
      creationTimestamp <string>
      deletionGracePeriodSeconds        <integer>
      deletionTimestamp <string>
      finalizers        <[]string>
      generateName      <string>
      generation        <integer>
      labels    <map[string]string>
      managedFields     <[]Object>
         apiVersion     <string>
         fieldsType     <string>
         fieldsV1       <map[string]>
         manager        <string>
         operation      <string>
         time   <string>
      name      <string>
      namespace <string>
      ownerReferences   <[]Object>
         apiVersion     <string>
         blockOwnerDeletion     <boolean>
         controller     <boolean>
         kind   <string>
         name   <string>
         uid    <string>
      resourceVersion   <string>
      selfLink  <string>
      uid       <string>
   spec <Object>
      activeDeadlineSeconds     <integer>
      affinity  <Object>
         nodeAffinity   <Object>
            preferredDuringSchedulingIgnoredDuringExecution     <[]Object>
               preference       <Object>
                  matchExpressions      <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
                  matchFields   <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
               weight   <integer>
            requiredDuringSchedulingIgnoredDuringExecution      <Object>
               nodeSelectorTerms        <[]Object>
                  matchExpressions      <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
                  matchFields   <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
         podAffinity    <Object>
            preferredDuringSchedulingIgnoredDuringExecution     <[]Object>
               podAffinityTerm  <Object>
                  labelSelector <Object>
                     matchExpressions   <[]Object>
                        key     <string>
                        operator        <string>
                        values  <[]string>
                     matchLabels        <map[string]string>
                  namespaces    <[]string>
                  topologyKey   <string>
               weight   <integer>
            requiredDuringSchedulingIgnoredDuringExecution      <[]Object>
               labelSelector    <Object>
                  matchExpressions      <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
                  matchLabels   <map[string]string>
               namespaces       <[]string>
               topologyKey      <string>
         podAntiAffinity        <Object>
            preferredDuringSchedulingIgnoredDuringExecution     <[]Object>
               podAffinityTerm  <Object>
                  labelSelector <Object>
                     matchExpressions   <[]Object>
                        key     <string>
                        operator        <string>
                        values  <[]string>
                     matchLabels        <map[string]string>
                  namespaces    <[]string>
                  topologyKey   <string>
               weight   <integer>
            requiredDuringSchedulingIgnoredDuringExecution      <[]Object>
               labelSelector    <Object>
                  matchExpressions      <[]Object>
                     key        <string>
                     operator   <string>
                     values     <[]string>
                  matchLabels   <map[string]string>
               namespaces       <[]string>
               topologyKey      <string>
      automountServiceAccountToken      <boolean>
      containers        <[]Object>
         args   <[]string>
         command        <[]string>
         env    <[]Object>
            name        <string>
            value       <string>
            valueFrom   <Object>
               configMapKeyRef  <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
               fieldRef <Object>
                  apiVersion    <string>
                  fieldPath     <string>
               resourceFieldRef <Object>
                  containerName <string>
                  divisor       <string>
                  resource      <string>
               secretKeyRef     <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
         envFrom        <[]Object>
            configMapRef        <Object>
               name     <string>
               optional <boolean>
            prefix      <string>
            secretRef   <Object>
               name     <string>
               optional <boolean>
         image  <string>
         imagePullPolicy        <string>
         lifecycle      <Object>
            postStart   <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
            preStop     <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
         livenessProbe  <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         name   <string>
         ports  <[]Object>
            containerPort       <integer>
            hostIP      <string>
            hostPort    <integer>
            name        <string>
            protocol    <string>
         readinessProbe <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         resources      <Object>
            limits      <map[string]string>
            requests    <map[string]string>
         securityContext        <Object>
            allowPrivilegeEscalation    <boolean>
            capabilities        <Object>
               add      <[]string>
               drop     <[]string>
            privileged  <boolean>
            procMount   <string>
            readOnlyRootFilesystem      <boolean>
            runAsGroup  <integer>
            runAsNonRoot        <boolean>
            runAsUser   <integer>
            seLinuxOptions      <Object>
               level    <string>
               role     <string>
               type     <string>
               user     <string>
            windowsOptions      <Object>
               gmsaCredentialSpec       <string>
               gmsaCredentialSpecName   <string>
               runAsUserName    <string>
         startupProbe   <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         stdin  <boolean>
         stdinOnce      <boolean>
         terminationMessagePath <string>
         terminationMessagePolicy       <string>
         tty    <boolean>
         volumeDevices  <[]Object>
            devicePath  <string>
            name        <string>
         volumeMounts   <[]Object>
            mountPath   <string>
            mountPropagation    <string>
            name        <string>
            readOnly    <boolean>
            subPath     <string>
            subPathExpr <string>
         workingDir     <string>
      dnsConfig <Object>
         nameservers    <[]string>
         options        <[]Object>
            name        <string>
            value       <string>
         searches       <[]string>
      dnsPolicy <string>
      enableServiceLinks        <boolean>
      ephemeralContainers       <[]Object>
         args   <[]string>
         command        <[]string>
         env    <[]Object>
            name        <string>
            value       <string>
            valueFrom   <Object>
               configMapKeyRef  <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
               fieldRef <Object>
                  apiVersion    <string>
                  fieldPath     <string>
               resourceFieldRef <Object>
                  containerName <string>
                  divisor       <string>
                  resource      <string>
               secretKeyRef     <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
         envFrom        <[]Object>
            configMapRef        <Object>
               name     <string>
               optional <boolean>
            prefix      <string>
            secretRef   <Object>
               name     <string>
               optional <boolean>
         image  <string>
         imagePullPolicy        <string>
         lifecycle      <Object>
            postStart   <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
            preStop     <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
         livenessProbe  <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         name   <string>
         ports  <[]Object>
            containerPort       <integer>
            hostIP      <string>
            hostPort    <integer>
            name        <string>
            protocol    <string>
         readinessProbe <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         resources      <Object>
            limits      <map[string]string>
            requests    <map[string]string>
         securityContext        <Object>
            allowPrivilegeEscalation    <boolean>
            capabilities        <Object>
               add      <[]string>
               drop     <[]string>
            privileged  <boolean>
            procMount   <string>
            readOnlyRootFilesystem      <boolean>
            runAsGroup  <integer>
            runAsNonRoot        <boolean>
            runAsUser   <integer>
            seLinuxOptions      <Object>
               level    <string>
               role     <string>
               type     <string>
               user     <string>
            windowsOptions      <Object>
               gmsaCredentialSpec       <string>
               gmsaCredentialSpecName   <string>
               runAsUserName    <string>
         startupProbe   <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         stdin  <boolean>
         stdinOnce      <boolean>
         targetContainerName    <string>
         terminationMessagePath <string>
         terminationMessagePolicy       <string>
         tty    <boolean>
         volumeDevices  <[]Object>
            devicePath  <string>
            name        <string>
         volumeMounts   <[]Object>
            mountPath   <string>
            mountPropagation    <string>
            name        <string>
            readOnly    <boolean>
            subPath     <string>
            subPathExpr <string>
         workingDir     <string>
      hostAliases       <[]Object>
         hostnames      <[]string>
         ip     <string>
      hostIPC   <boolean>
      hostNetwork       <boolean>
      hostPID   <boolean>
      hostname  <string>
      imagePullSecrets  <[]Object>
         name   <string>
      initContainers    <[]Object>
         args   <[]string>
         command        <[]string>
         env    <[]Object>
            name        <string>
            value       <string>
            valueFrom   <Object>
               configMapKeyRef  <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
               fieldRef <Object>
                  apiVersion    <string>
                  fieldPath     <string>
               resourceFieldRef <Object>
                  containerName <string>
                  divisor       <string>
                  resource      <string>
               secretKeyRef     <Object>
                  key   <string>
                  name  <string>
                  optional      <boolean>
         envFrom        <[]Object>
            configMapRef        <Object>
               name     <string>
               optional <boolean>
            prefix      <string>
            secretRef   <Object>
               name     <string>
               optional <boolean>
         image  <string>
         imagePullPolicy        <string>
         lifecycle      <Object>
            postStart   <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
            preStop     <Object>
               exec     <Object>
                  command       <[]string>
               httpGet  <Object>
                  host  <string>
                  httpHeaders   <[]Object>
                     name       <string>
                     value      <string>
                  path  <string>
                  port  <string>
                  scheme        <string>
               tcpSocket        <Object>
                  host  <string>
                  port  <string>
         livenessProbe  <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         name   <string>
         ports  <[]Object>
            containerPort       <integer>
            hostIP      <string>
            hostPort    <integer>
            name        <string>
            protocol    <string>
         readinessProbe <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         resources      <Object>
            limits      <map[string]string>
            requests    <map[string]string>
         securityContext        <Object>
            allowPrivilegeEscalation    <boolean>
            capabilities        <Object>
               add      <[]string>
               drop     <[]string>
            privileged  <boolean>
            procMount   <string>
            readOnlyRootFilesystem      <boolean>
            runAsGroup  <integer>
            runAsNonRoot        <boolean>
            runAsUser   <integer>
            seLinuxOptions      <Object>
               level    <string>
               role     <string>
               type     <string>
               user     <string>
            windowsOptions      <Object>
               gmsaCredentialSpec       <string>
               gmsaCredentialSpecName   <string>
               runAsUserName    <string>
         startupProbe   <Object>
            exec        <Object>
               command  <[]string>
            failureThreshold    <integer>
            httpGet     <Object>
               host     <string>
               httpHeaders      <[]Object>
                  name  <string>
                  value <string>
               path     <string>
               port     <string>
               scheme   <string>
            initialDelaySeconds <integer>
            periodSeconds       <integer>
            successThreshold    <integer>
            tcpSocket   <Object>
               host     <string>
               port     <string>
            timeoutSeconds      <integer>
         stdin  <boolean>
         stdinOnce      <boolean>
         terminationMessagePath <string>
         terminationMessagePolicy       <string>
         tty    <boolean>
         volumeDevices  <[]Object>
            devicePath  <string>
            name        <string>
         volumeMounts   <[]Object>
            mountPath   <string>
            mountPropagation    <string>
            name        <string>
            readOnly    <boolean>
            subPath     <string>
            subPathExpr <string>
         workingDir     <string>
      nodeName  <string>
      nodeSelector      <map[string]string>
      overhead  <map[string]string>
      preemptionPolicy  <string>
      priority  <integer>
      priorityClassName <string>
      readinessGates    <[]Object>
         conditionType  <string>
      restartPolicy     <string>
      runtimeClassName  <string>
      schedulerName     <string>
      securityContext   <Object>
         fsGroup        <integer>
         fsGroupChangePolicy    <string>
         runAsGroup     <integer>
         runAsNonRoot   <boolean>
         runAsUser      <integer>
         seLinuxOptions <Object>
            level       <string>
            role        <string>
            type        <string>
            user        <string>
         supplementalGroups     <[]integer>
         sysctls        <[]Object>
            name        <string>
            value       <string>
         windowsOptions <Object>
            gmsaCredentialSpec  <string>
            gmsaCredentialSpecName      <string>
            runAsUserName       <string>
      serviceAccount    <string>
      serviceAccountName        <string>
      shareProcessNamespace     <boolean>
      subdomain <string>
      terminationGracePeriodSeconds     <integer>
      tolerations       <[]Object>
         effect <string>
         key    <string>
         operator       <string>
         tolerationSeconds      <integer>
         value  <string>
      topologySpreadConstraints <[]Object>
         labelSelector  <Object>
            matchExpressions    <[]Object>
               key      <string>
               operator <string>
               values   <[]string>
            matchLabels <map[string]string>
         maxSkew        <integer>
         topologyKey    <string>
         whenUnsatisfiable      <string>
      volumes   <[]Object>
         awsElasticBlockStore   <Object>
            fsType      <string>
            partition   <integer>
            readOnly    <boolean>
            volumeID    <string>
         azureDisk      <Object>
            cachingMode <string>
            diskName    <string>
            diskURI     <string>
            fsType      <string>
            kind        <string>
            readOnly    <boolean>
         azureFile      <Object>
            readOnly    <boolean>
            secretName  <string>
            shareName   <string>
         cephfs <Object>
            monitors    <[]string>
            path        <string>
            readOnly    <boolean>
            secretFile  <string>
            secretRef   <Object>
               name     <string>
            user        <string>
         cinder <Object>
            fsType      <string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
            volumeID    <string>
         configMap      <Object>
            defaultMode <integer>
            items       <[]Object>
               key      <string>
               mode     <integer>
               path     <string>
            name        <string>
            optional    <boolean>
         csi    <Object>
            driver      <string>
            fsType      <string>
            nodePublishSecretRef        <Object>
               name     <string>
            readOnly    <boolean>
            volumeAttributes    <map[string]string>
         downwardAPI    <Object>
            defaultMode <integer>
            items       <[]Object>
               fieldRef <Object>
                  apiVersion    <string>
                  fieldPath     <string>
               mode     <integer>
               path     <string>
               resourceFieldRef <Object>
                  containerName <string>
                  divisor       <string>
                  resource      <string>
         emptyDir       <Object>
            medium      <string>
            sizeLimit   <string>
         fc     <Object>
            fsType      <string>
            lun <integer>
            readOnly    <boolean>
            targetWWNs  <[]string>
            wwids       <[]string>
         flexVolume     <Object>
            driver      <string>
            fsType      <string>
            options     <map[string]string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
         flocker        <Object>
            datasetName <string>
            datasetUUID <string>
         gcePersistentDisk      <Object>
            fsType      <string>
            partition   <integer>
            pdName      <string>
            readOnly    <boolean>
         gitRepo        <Object>
            directory   <string>
            repository  <string>
            revision    <string>
         glusterfs      <Object>
            endpoints   <string>
            path        <string>
            readOnly    <boolean>
         hostPath       <Object>
            path        <string>
            type        <string>
         iscsi  <Object>
            chapAuthDiscovery   <boolean>
            chapAuthSession     <boolean>
            fsType      <string>
            initiatorName       <string>
            iqn <string>
            iscsiInterface      <string>
            lun <integer>
            portals     <[]string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
            targetPortal        <string>
         name   <string>
         nfs    <Object>
            path        <string>
            readOnly    <boolean>
            server      <string>
         persistentVolumeClaim  <Object>
            claimName   <string>
            readOnly    <boolean>
         photonPersistentDisk   <Object>
            fsType      <string>
            pdID        <string>
         portworxVolume <Object>
            fsType      <string>
            readOnly    <boolean>
            volumeID    <string>
         projected      <Object>
            defaultMode <integer>
            sources     <[]Object>
               configMap        <Object>
                  items <[]Object>
                     key        <string>
                     mode       <integer>
                     path       <string>
                  name  <string>
                  optional      <boolean>
               downwardAPI      <Object>
                  items <[]Object>
                     fieldRef   <Object>
                        apiVersion      <string>
                        fieldPath       <string>
                     mode       <integer>
                     path       <string>
                     resourceFieldRef   <Object>
                        containerName   <string>
                        divisor <string>
                        resource        <string>
               secret   <Object>
                  items <[]Object>
                     key        <string>
                     mode       <integer>
                     path       <string>
                  name  <string>
                  optional      <boolean>
               serviceAccountToken      <Object>
                  audience      <string>
                  expirationSeconds     <integer>
                  path  <string>
         quobyte        <Object>
            group       <string>
            readOnly    <boolean>
            registry    <string>
            tenant      <string>
            user        <string>
            volume      <string>
         rbd    <Object>
            fsType      <string>
            image       <string>
            keyring     <string>
            monitors    <[]string>
            pool        <string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
            user        <string>
         scaleIO        <Object>
            fsType      <string>
            gateway     <string>
            protectionDomain    <string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
            sslEnabled  <boolean>
            storageMode <string>
            storagePool <string>
            system      <string>
            volumeName  <string>
         secret <Object>
            defaultMode <integer>
            items       <[]Object>
               key      <string>
               mode     <integer>
               path     <string>
            optional    <boolean>
            secretName  <string>
         storageos      <Object>
            fsType      <string>
            readOnly    <boolean>
            secretRef   <Object>
               name     <string>
            volumeName  <string>
            volumeNamespace     <string>
         vsphereVolume  <Object>
            fsType      <string>
            storagePolicyID     <string>
            storagePolicyName   <string>
            volumePath  <string>
   status       <Object>
      conditions        <[]Object>
         lastProbeTime  <string>
         lastTransitionTime     <string>
         message        <string>
         reason <string>
         status <string>
         type   <string>
      containerStatuses <[]Object>
         containerID    <string>
         image  <string>
         imageID        <string>
         lastState      <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
         name   <string>
         ready  <boolean>
         restartCount   <integer>
         started        <boolean>
         state  <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
      ephemeralContainerStatuses        <[]Object>
         containerID    <string>
         image  <string>
         imageID        <string>
         lastState      <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
         name   <string>
         ready  <boolean>
         restartCount   <integer>
         started        <boolean>
         state  <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
      hostIP    <string>
      initContainerStatuses     <[]Object>
         containerID    <string>
         image  <string>
         imageID        <string>
         lastState      <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
         name   <string>
         ready  <boolean>
         restartCount   <integer>
         started        <boolean>
         state  <Object>
            running     <Object>
               startedAt        <string>
            terminated  <Object>
               containerID      <string>
               exitCode <integer>
               finishedAt       <string>
               message  <string>
               reason   <string>
               signal   <integer>
               startedAt        <string>
            waiting     <Object>
               message  <string>
               reason   <string>
      message   <string>
      nominatedNodeName <string>
      phase     <string>
      podIP     <string>
      podIPs    <[]Object>
         ip     <string>
      qosClass  <string>
      reason    <string>
      startTime <string>

```
</details>

and how would get more details about specifics

<details>
<summary>kubectl explain pod.spec.containers </summary>

```
$ kubectl explain pod.spec.containers

KIND:     Pod
VERSION:  v1

RESOURCE: containers <[]Object>

DESCRIPTION:
     List of containers belonging to the pod. Containers cannot currently be
     added or removed. There must be at least one container in a Pod. Cannot be
     updated.

     A single application container that you want to run within a pod.

FIELDS:
   args <[]string>
     Arguments to the entrypoint. The docker image's CMD is used if this is not
     provided. Variable references $(VAR_NAME) are expanded using the
     container's environment. If a variable cannot be resolved, the reference in
     the input string will be unchanged. The $(VAR_NAME) syntax can be escaped
     with a double $$, ie: $$(VAR_NAME). Escaped references will never be
     expanded, regardless of whether the variable exists or not. Cannot be
     updated. More info:
     https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#running-a-command-in-a-shell

   command      <[]string>
     Entrypoint array. Not executed within a shell. The docker image's
     ENTRYPOINT is used if this is not provided. Variable references $(VAR_NAME)
     are expanded using the container's environment. If a variable cannot be
     resolved, the reference in the input string will be unchanged. The
     $(VAR_NAME) syntax can be escaped with a double $$, ie: $$(VAR_NAME).
     Escaped references will never be expanded, regardless of whether the
     variable exists or not. Cannot be updated. More info:
     https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#running-a-command-in-a-shell

   env  <[]Object>
     List of environment variables to set in the container. Cannot be updated.

   envFrom      <[]Object>
     List of sources to populate environment variables in the container. The
     keys defined within a source must be a C_IDENTIFIER. All invalid keys will
     be reported as an event when the container is starting. When a key exists
     in multiple sources, the value associated with the last source will take
     precedence. Values defined by an Env with a duplicate key will take
     precedence. Cannot be updated.

   image        <string>
     Docker image name. More info:
     https://kubernetes.io/docs/concepts/containers/images This field is
     optional to allow higher level config management to default or override
     container images in workload controllers like Deployments and StatefulSets.

   imagePullPolicy      <string>
     Image pull policy. One of Always, Never, IfNotPresent. Defaults to Always
     if :latest tag is specified, or IfNotPresent otherwise. Cannot be updated.
     More info:
     https://kubernetes.io/docs/concepts/containers/images#updating-images

   lifecycle    <Object>
     Actions that the management system should take in response to container
     lifecycle events. Cannot be updated.

   livenessProbe        <Object>
     Periodic probe of container liveness. Container will be restarted if the
     probe fails. Cannot be updated. More info:
     https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle#container-probes

   name <string> -required-
     Name of the container specified as a DNS_LABEL. Each container in a pod
     must have a unique name (DNS_LABEL). Cannot be updated.

   ports        <[]Object>
     List of ports to expose from the container. Exposing a port here gives the
     system additional information about the network connections a container
     uses, but is primarily informational. Not specifying a port here DOES NOT
     prevent that port from being exposed. Any port which is listening on the
     default "0.0.0.0" address inside a container will be accessible from the
     network. Cannot be updated.

   readinessProbe       <Object>
     Periodic probe of container service readiness. Container will be removed
     from service endpoints if the probe fails. Cannot be updated. More info:
     https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle#container-probes

   resources    <Object>
     Compute Resources required by this container. Cannot be updated. More info:
     https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/

   securityContext      <Object>
     Security options the pod should run with. More info:
     https://kubernetes.io/docs/concepts/policy/security-context/ More info:
     https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

   startupProbe <Object>
     StartupProbe indicates that the Pod has successfully initialized. If
     specified, no other probes are executed until this completes successfully.
     If this probe fails, the Pod will be restarted, just as if the
     livenessProbe failed. This can be used to provide different probe
     parameters at the beginning of a Pod's lifecycle, when it might take a long
     time to load data or warm a cache, than during steady-state operation. This
     cannot be updated. This is a beta feature enabled by the StartupProbe
     feature flag. More info:
     https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle#container-probes

   stdin        <boolean>
     Whether this container should allocate a buffer for stdin in the container
     runtime. If this is not set, reads from stdin in the container will always
     result in EOF. Default is false.

   stdinOnce    <boolean>
     Whether the container runtime should close the stdin channel after it has
     been opened by a single attach. When stdin is true the stdin stream will
     remain open across multiple attach sessions. If stdinOnce is set to true,
     stdin is opened on container start, is empty until the first client
     attaches to stdin, and then remains open and accepts data until the client
     disconnects, at which time stdin is closed and remains closed until the
     container is restarted. If this flag is false, a container processes that
     reads from stdin will never receive an EOF. Default is false

   terminationMessagePath       <string>
     Optional: Path at which the file to which the container's termination
     message will be written is mounted into the container's filesystem. Message
     written is intended to be brief final status, such as an assertion failure
     message. Will be truncated by the node if greater than 4096 bytes. The
     total message length across all containers will be limited to 12kb.
     Defaults to /dev/termination-log. Cannot be updated.

   terminationMessagePolicy     <string>
     Indicate how the termination message should be populated. File will use the
     contents of terminationMessagePath to populate the container status message
     on both success and failure. FallbackToLogsOnError will use the last chunk
     of container log output if the termination message file is empty and the
     container exited with an error. The log output is limited to 2048 bytes or
     80 lines, whichever is smaller. Defaults to File. Cannot be updated.

   tty  <boolean>
     Whether this container should allocate a TTY for itself, also requires
     'stdin' to be true. Default is false.

   volumeDevices        <[]Object>
     volumeDevices is the list of block devices to be used by the container.

   volumeMounts <[]Object>
     Pod volumes to mount into the container's filesystem. Cannot be updated.

   workingDir   <string>
     Container's working directory. If not specified, the container runtime's
     default will be used, which might be configured in the container image.
     Cannot be updated.

```
</details>

So whenever in exam like [CKA][3]/[CKAD][4], though you have access to Kubernetes [Documentation][5] but if you are not sure about what spec a particular resource has or what fields it has you can use `kubectl explain`, it is the much faster than searching through docs.

[1]: https://kubernetes.io/docs/reference/kubectl/overview/
[2]: https://kubernetes.io/
[3]: https://www.cncf.io/certification/cka/
[4]: https://www.cncf.io/certification/ckad/
[5]: https://kubernetes.io/docs/