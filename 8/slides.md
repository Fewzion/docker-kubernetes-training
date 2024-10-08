layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/citeops-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 8
## August 23rd 2021

---

### 8.1 Minikube Status

- Check the status of minikube: `minikube status`
- If not running: `minikube start`
- Get your k8s dashboard up: `minikube dashboard`

---

### 8.2 Rebuild+push our frontend from Session 6:

- `docker build -t session8:flask .`
- `docker login`
- `docker tag session8:flask kevinciq/session8:flask`
- `docker push kevinciq/session8:flask`
- sources here: https://github.com/fewzion/docker-kubernetes-training/tree/main/8/cluster/flask

---

### 8.3 Create the flask deployment

- edit `flask-deployment.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask
spec:
  replicas: 3
  selector:
    matchLabels:
        app: session8
        tier: flask
  template:
    metadata:
      labels:
        app: session8
        tier: flask
    spec:
      containers:
      - name: flaskapp
        image: session8:flask
        ports:
        - containerPort: 80

```

---

### 8.4 Apply the flask deployment

- Run `kubectl apply -f flask-deployment.yaml`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.dashboard.flask.png)

---

### 8.5 Create the flask service

- Edit `flask-service.yaml`:

```
apiVersion: v1
kind: Service
metadata:
  name: flask
  labels:
    app: session8
    tier: flask
spec:
  type: ClusterIP
  selector:
    app: session8
    tier: flask
  ports:
  - port: 5000

```

---

### 8.6 Apply the flask service

- Run `kubectl apply -f flask-service.yaml`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.dashboard.flask.service.png)

---

### 8.7 Backend

- Edit `redis-deployment.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: session8
      tier: redis
  template:
    metadata:
      labels:
        app: session8
        tier: redis
    spec:
      containers:
      - name: redis
        image: "redis:alpine"
        ports:
        - containerPort: 6379      
```

---

### 8.8 Backend Service

- Edit `redis-service.yaml`:

```
apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: session8
    tier: redis
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: session8
    tier: redis
```

---

### 8.9 Launch Redis Backend

- `kubectl apply -f redis-deployment.yaml,redis-service.yaml`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.dashboard.services.png)

---

### 8.10 Service Info

- `kubectl get service flask`:
- `kubectl get service redis`:

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.kubectl.services.png)

---

### 8.11 Port Forward

- `kubectl port-forward svc/flask 8080:5000`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.flask.forwarded.png)

---

### 8.12 Flask Logs

- Run `kubectl logs -f svc/flask`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.flask.logs.png)

---

### 8.13 Accessing a Pod

- Run `kubectl get pods`:

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.kubectl.get.pods.png)

---

### 8.14 Accessing a Pod

- Run `kubectl exec -it flask-76c47d8bbd-cttdm -- sh`
- You should get a '#' prompt once you're shelled in
- Run `apt-get update`
- Run `apt-get install curl`
- Run `curl 127.0.0.1:5000`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.flask.curl.png)

---

### 8.15 Scaling

- Run `kubectl scale deployment flask --replicas=5`

![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/k8s.flask.scale.png)

---

# No 'Homework' for Session 8

- I'm on leave next week

# Session 9 will be September 6th

- Keep safe

