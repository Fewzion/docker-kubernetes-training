layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/citeops-background.png)
background-size: contain

---

### CW Docker+Kubernetes Training
### Session 1 - Intro to CW's new Cluster Platform
### Date TBA

Welcome!

This first session is an introduction to Commit Works' new Kubernetes platform.

Topics:
- What we're building
- Why we're building it
- What tools and technologies we're using

---

### 1.1 Topics

- What we're building
  - Cloud Native
  - Kubernetes Clusters
  - CiteOps Docker Containers
  - Build pipelines
- Why we're building it
  - Cybersecurity
  - Scalability
  - Availability/Resilience (self healing, failover, multiple AZs)
  - Costs
  - Why not just two Linux boxes?
- What tools and technologies we're using
  - AWS EKS
  - Terraform
  - Docker
  - IaC
  - GitHub Actions
  - Prometheus/Grafana
  - Traefik
  - Kubernetes Web Dashboard

---

### 1.2 What we're building - Cloud Native

We are building a "cloud native" platform.

What is Cloud Native?

*Cloud native is the software approach of building, deploying, and managing modern applications in cloud computing environments. Modern companies want to build highly scalable, flexible, and resilient applications that they can update quickly to meet customer demands. To do so, they use modern tools and techniques that inherently support application development on cloud infrastructure. These cloud-native technologies support fast and frequent changes to applications without impacting service delivery, providing adopters with an innovative, competitive advantage.*

source: https://aws.amazon.com/what-is/cloud-native/

---

### 1.3 What we're building - Cloud Native

*Cloud native is about speed and agility. Business systems are evolving from enabling business capabilities to weapons of strategic transformation that accelerate business velocity and growth. It's imperative to get new ideas to market immediately.*

*At the same time, business systems have also become increasingly complex with users demanding more. They expect rapid responsiveness, innovative features, and zero downtime. Performance problems, recurring errors, and the inability to move fast are no longer acceptable. Your users will visit your competitor. Cloud-native systems are designed to embrace rapid change, large scale, and resilience.*

**Netflix** Has 600+ services in production. Deploys 100 times per day.

**Uber**  Has 1,000+ services in production. Deploys several thousand times each week.

**WeChat**  Has 3,000+ services in production. Deploys 1,000 times a day.

source: https://learn.microsoft.com/en-us/dotnet/architecture/cloud-native/definition

---

### 1.4 What we're building - Kubernetes Clusters

We are building our cloud native platform as Kubernetes (k8s) clusters.

What is Kubernetes?

*Kubernetes automates operational tasks of container management and includes built-in commands for deploying applications, rolling out changes to your applications, scaling your applications up and down to fit changing needs, monitoring your applications, and more — making it easier to manage applications.*

source: https://cloud.google.com/learn/what-is-kubernetes

---

### 1.5 What we're building - Kubernetes Clusters

What are the benefits of Kubernetes?

**Automated operations** *Kubernetes has built-in commands to handle a lot of the heavy lifting that goes into application management, allowing you to automate day-to-day operations. You can make sure applications are always running the way you intended them to run.*

**Infrastructure abstraction** *When you install Kubernetes, it handles the compute, networking, and storage on behalf of your workloads. This allows developers to focus on applications and not worry about the underlying environment.*

**Service health monitoring** *Kubernetes continuously runs health checks against your services, restarting containers that fail, or have stalled, and only making available services to users when it has confirmed they are running.*

source: https://cloud.google.com/learn/what-is-kubernetes

---

### 1.6 What we're building - Kubernetes Clusters

.center[![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/CiteOps.Kubernetes.Architecture-AWS.png)]

---

### 1.7 What we're building - CiteOps Docker Containers


.center[![](https://raw.githubusercontent.com/fewzion/docker-kubernetes-training/main/img/server.ghcr.png)]

---

### 1.7.1 What we're building - CiteOps Docker Containers

- Windows/IIS
  - We build .nupkg files that contain our .NET DLLs, front end javascript files, etc
  - Octopus then deploys it where we tell it to
- DockerK8S
  - We build container images that contain our .NET DLLs, front end javascript files, etc
  - k8s (kubectl) then deploys it where we tell it to

---

### 1.7.2 What we're building - CiteOps Docker Containers

Whats in the box? (package)
- Windows/IIS
  - .nupkg file (just a .zip)
  - Built from a .nuspec file - `./Fewzion/Fewzion.nuspec` in the server repo
  - `<file src="FOUT\**\*.*" target="" />`
  - In TeamCity we publish `Fewzion\Fewzion.csproj` to `Fewzion\FOUT`
- Docker/K8S
  - container file (not just a .zip)
  - Built from a `Dockerfile` file - `./Dockerfile` in the root of the server repo
  - We use GitHub Actions instead of TeamCity to build it
  - `.github/workflows/docker-ci.yml` in the server repo
  - `dotnet publish Fewzion/Fewzion.csproj -c Release --no-restore -o Fewzion/out`
  - `cp -r Fewzion/out/* context/CiteOps/`
  - `- name: Docker Build & Push`
  - `uses: mr-smithers-excellent/docker-build-push@v5`
  - `directory: ./context`

---

### 1.8 What we're building - Build pipelines

---

# Next Session

- TBA
