---
title: Kubernetes Pods with YAML
description: Learn to write and test Kubernetes Pods with YAML
---

## Step-01: Kubernetes YAML Top level Objects
- Discuss about the k8s YAML top level objects
- **kube-base-definition.yml**
```yml
apiVersion:
kind:
metadata:
  
spec:
```
- [Kubernetes Reference](https://kubernetes.io/docs/reference/)
- [Kubernetes API Reference](https://kubernetes.io/docs/reference/kubernetes-api/)
-  [Pod API Objects Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#pod-v1-core)

## Step-02: Create Simple Pod Definition using YAML 
- We are going to create a very basic pod definition
- **01-pod-definition.yml**
```yaml
apiVersion: v1 # String
kind: Pod  # String
metadata: # Dictionary
  name: myapp-pod
  labels: # Dictionary 
    app: myapp         
spec:
  containers: # List
    - name: myapp
      image: nholuongut/kubenginx:1.0.0
      ports:
        - containerPort: 80
```
- **Create Pod**
```t
# Change Directory
cd kube-manifests

# Create Pod
kubectl create -f 01-pod-definition.yml
[or]
kubectl apply -f 01-pod-definition.yml

# List Pods
kubectl get pods
```

## Step-03: Create a LoadBalancer Service
- **02-pod-LoadBalancer-service.yml**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-pod-loadbalancer-service  # Name of the Service
spec:
  type: LoadBalancer
  selector:
  # Loadbalance traffic across Pods matching this label selector
    app: myapp
  # Accept traffic sent to port 80    
  ports: 
    - name: http
      port: 80    # Service Port
      targetPort: 80 # Container Port
```
- **Create LoadBalancer Service for Pod**
```t
# Create Service
kubectl apply -f 02-pod-LoadBalancer-service.yml

# List Service
kubectl get svc

# Access Application
http://<Load-Balancer-Service-IP>
curl http://<Load-Balancer-Service-IP>
```

## Step-04: Clean-Up Kubernetes Pod and Service
```t
# Change Directory
cd kube-manifests

# Delete Pod
kubectl delete -f 01-pod-definition.yml

# Delete Service
kubectl delete -f  02-pod-LoadBalancer-service.yml
```


## API Object References
- [Kubernetes API Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/)
- [Pod Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#pod-v1-core)
- [Service Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#service-v1-core)
- [Kubernetes API Reference](https://kubernetes.io/docs/reference/kubernetes-api/)


