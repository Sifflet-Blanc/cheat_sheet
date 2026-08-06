**[< Home](README.md)**

# Kubernetes

V1 done by Google then given to CNSF as an opensource project 

## Installation
```shell
curl -LO "https://dl.k8s.io/release/v1.34.9/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```


## Architecture 


## Resource type 
- [Pod](#pod) 
- [ReplicaSet](#replicaset) 
- [Deployment](#deployment) 
- Namespace
- [Service](#service) 
- [Ingress](#ingress)
- [Secret](#secret)
- [ConfigMap](#configmap)
- hpa
- [NetworkPolicy](#networkpolicy)
- [PersistentVolumeClaim](#persistentvolumeclaim)

### Pod
Run a pod
The imperative way
```shell
kubectl run mynginx --image registry.takima.io/school/proxy/nginx
```
or
The declarative way (better)
```shell
kubectl apply -f unicorn-front-pod.yml
```

Observe the state of the pod
```shell
kubectl get pods mynginx
```

Connect to a pod
```shell
kubectl exec name_of_pod -it -- bash
```

See the logs of a pod
```shell
kubectl logs name_of_pod
```

Delete a pod
```shell
kubectl delete pods name_of_pod
kubectl delete --all pods
```

### all 
```shell
kubectl get all
```
```shell
kubectl delete all --all
```

### Replicaset
The command of the replicaset is the same as the pod
Exemple :
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: unicorn-front-replicaset
  labels:
    app: unicorn-front
spec:
  template:
    metadata:
      name: unicorn-front-pod
      labels:
        app: unicorn-front
    spec:
      containers:
        - name: unicorn-front
          image: registry.takima.io/school/proxy/nginx
  replicas: 3
  selector:
    matchLabels:
      app: unicorn-front
```
### Deployment
The command of the deployment is the same as the pod

Follow the deployment of a new version
```shell
kubectl rollout status deployment.v1.apps/unicorn-front-deployment
```

Explain it
```shell
kubectl describe deployments
```

```shell
kubectl rollout history deployment.v1.apps/unicorn-front-deployment
```

We can check the detail of the revision :
```shell
kubectl rollout history deployment.v1.apps/unicorn-front-deployment --revision=2
```

And go back to it with :
```shell
kubectl rollout undo deployment.v1.apps/unicorn-front-deployment --to-revision=2
```

We can also do
```shell
kubectl rollout undo deployment.v1.apps/unicorn-front-deployment
```

We can scale the deployment :
```shell
kubectl scale deployment.v1.apps/unicorn-front-deployment --replicas=5
```

Exemple :
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: unicorn-front-deployment
  labels:
    app: unicorn-front
spec:
  replicas: 3
  selector:
    matchLabels:
      app: unicorn-front
  template:
    metadata:
      labels:
        app: unicorn-front
    spec:
      containers:
      - name: unicorn-front
        image: registry.takima.io/school/proxy/nginx:1.7.9
        ports:
        - containerPort: 80
```


### Service
The service is the way to communicate between pods.  (it will handle load balancing for exemple)

Exemple :
```yaml
apiVersion: v1
kind: Service
metadata:
  name: unicorn-front-service
spec:
  selector:
    app: unicorn-front
  ports:  
    - protocol: TCP
      port: 80
      targetPort: 80
```

### Ingress
The ingress is the way to communicate between pods and the internet
Exemple :
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: unicorn-front-ingress
spec:
  ingressClassName: nginx
  rules:
    - host: replace-with-your-url
      http:
        paths:
          - backend:
              service:
                name: unicorn-front-service
                port:
                  number: 80
            path: /
            pathType: Prefix
```

### HPA
Horizontal Pod Autoscaler

```shell
kubectl describe hpa name_of_hpa
```

### ConfigMap
Ex :
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: web-app
data:
  # property-like keys; each key maps to a simple value
  color: "#200"
```

### Secret
Ex :
```echo -n 'test123*' | base64```
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: hello-secret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

### NetworkPolicy
Ex :
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-nginx
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: "true"
```

### PersistentVolumeClaim

Ex :
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pg-db
spec:
  storageClassName: gp3
  accessModes:
  - ReadWriteOnce
  volumeMode: Filesystem
  resources:
     requests:
         storage: 3Gi
```


## Healthcheck
liveness (restarted if it fails)

readiness (don't receive new connections if it fails)
