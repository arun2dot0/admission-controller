## Kubernetes Admission Controller

Kubernetes admission controllers, can be effectively used to prevent privileged or insecure pods from being loaded. 
How do you want to use it

### Enable Pod Security Admission:
Ensure that the Pod Security Admission controller is enabled in your cluster. It's available by default from Kubernetes v1.23 onwards.

### Configure Pod Security Standards
Use the built-in Pod Security Standards to enforce security policies. There are three levels:
 - Privileged (least secure)
 - Baseline
 - Restricted (most secure)

### Demo 

This is a simple demo to enforce baseline and show how it can stop a priviled container from being loaded

Create a namespace test 

```kubectl create namespace test```

Create specification that will enforce privileged containers to be not loaded

```kubectl label --overwrite ns test pod-security.kubernetes.io/enforce=baseline```

Validate 

```kubectl apply -f test-privileged-deployment.yaml ```

You should see the following error

```
Error from server (Forbidden): error when creating "test-privileged-deployment.yaml": pods "nginx-priv" is forbidden: violates PodSecurity "baseline:latest": privileged (container "nginx-priv" must not set securityContext.privileged=true)
```

This you want to enforce this globally , I have provided the AdmissionConfiguration file that can be used 

Specify this configuration file via the --admission-control-config-file parameter to the kube-apiserver.


### Wait There is More ...
Admission controller can be used for a variety of other things 

#### Validate Image sources
Create policies to allow pulling images only from approved registries

#### Don't load Images with Vulnerabilities
Create policies to not load images that has images with high risk vulnerabilities


