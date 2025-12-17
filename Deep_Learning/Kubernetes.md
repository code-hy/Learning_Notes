# Workshop Notes

This video walks through taking an existing ML model, wrapping it in a FastAPI web service, containerizing it with Docker, and deploying it on Kubernetes with Horizontal Pod Autoscaling so it can scale under load. It uses example code from the ML Zoomcamp repository and a dedicated workshop repo to show a practical, production-style deployment pattern.  Alexey went thru the entire process of deployment using Kubernetes and docker.

## Kubernetes
Also known as K8s, is an open-source system for automating deployment, scaling, and management of containerised applications.  So Kubernetes manages 'dockers'
<img width="1262" height="680" alt="image" src="https://github.com/user-attachments/assets/6ac3825b-58d8-4307-851f-67da040dd39a" />

Node is equivalent to a computer/server or EC2 instance in AWS.
Each Node has PODS, which is a container that runs a specific images... each node can have one or more pods...group pods in deployments
<img width="1218" height="777" alt="image" src="https://github.com/user-attachments/assets/f1bdba0e-7b92-4baf-b01c-05503b1da46d" />

within one deployment, they have the same image...same parameters, same image configuration, same environment variables

<img width="1165" height="717" alt="image" src="https://github.com/user-attachments/assets/5c783f32-2c4e-4227-a662-bb05842c4bd6" />

larger nodes for tensor-flow serving model TF-Serving Deployment, so pods are larger

<img width="1163" height="682" alt="image" src="https://github.com/user-attachments/assets/2d74eae8-4ddb-4626-9de8-c85fd8eb8480" />

POD - docker container that runs on a node.

Deployment - group of PODS with the same image and configuration.

Services - gateway service, and tensor flow model service,  service is entry point to our deployment..service responsible for routing the request. service sents request to any available pod.


<img width="1198" height="725" alt="image" src="https://github.com/user-attachments/assets/ce76694b-2e8a-4e1e-9c16-761c69171f98" />

<img width="1251" height="719" alt="image" src="https://github.com/user-attachments/assets/729a7f36-d9cf-4ae5-8ffb-d704e3d3e339" />

1. user sends request to gateway service
2. gateway service routes to a gateway pod
3. gateway pod then sends it to the model service
4. model service sends it to the TF-Serving Pod
5. TF-serving pod then sends result back to model service
6. model service then sends it back to the gateway pod
7. gateway pod then sends it to the gateway service
8. gateway service sends results back to the user

<img width="1128" height="474" alt="image" src="https://github.com/user-attachments/assets/af9834d6-897e-4410-87ea-bc6aa655788a" />

gateway service should be external, and model -service is internal..

<img width="1100" height="736" alt="image" src="https://github.com/user-attachments/assets/c2b8b776-561c-472a-a045-a487c213b409" />

<img width="1167" height="623" alt="image" src="https://github.com/user-attachments/assets/7d4e97db-fccb-43df-aabb-e74bc75e3645" />


9. 

<img width="1063" height="742" alt="image" src="https://github.com/user-attachments/assets/5c8f13a9-1b95-4394-bbe7-c01b71fa2a68" />

INGRESS - entry point to the cluster

Bunch of clients, to deal with load, Kubernetes can start up more pods... automatically scale up deployment when load increases... HPA  - horizontal pod autoscaler...

<img width="945" height="702" alt="image" src="https://github.com/user-attachments/assets/dc4dae6e-7001-46f7-bffc-dfc7ab2ceb45" />


## High-level summary

- The instructor starts from a trained model from Machine Learning Zoomcamp and turns it into an HTTP API using FastAPI with a `/predict` endpoint.  
- The FastAPI app, its dependencies, and the model artifacts are packaged into a Docker image and tested locally.  
- The image is pushed to a registry and deployed to a Kubernetes cluster using Deployment and Service manifests, then scaled dynamically with an HPA while observing behavior live.  

## Step-by-step process (conceptual)

1. Pick a trained model and define clear input/output schemas for predictions. Expose them via FastAPI routes.   
2. Write a `Dockerfile` that installs Python, copies the app and model, installs dependencies, and starts the FastAPI server (usually via Uvicorn).   
3. Build the Docker image locally, run it with `docker run`, and hit the prediction endpoint to verify everything works.   
4. Push the Docker image to a container registry (e.g., Docker Hub or a cloud registry) that your Kubernetes cluster can access.   
5. Create a Kubernetes Deployment manifest that references the image, sets environment variables, and configures replicas, resource requests, and limits.   
6. Create a Kubernetes Service (ClusterIP/LoadBalancer/Ingress) to expose the FastAPI pods inside or outside the cluster and route traffic to them.   
7. Apply the manifests with `kubectl apply`, then check pods, logs, and service endpoints using `kubectl get` and `kubectl logs`.   
8. Configure a Horizontal Pod Autoscaler that scales replicas based on CPU (or another metric), then generate load to see pods scale up and down.   
9. Iterate: adjust resource limits, replicas, or app code, rebuild/push the image, and re-apply manifests as needed for a robust deployment.

[1](https://www.youtube.com/live/c_CzCsCnWoU)

# Key Points
This workshop shows how to take an ML model, wrap it in a FastAPI service, containerize it with Docker, and run it on Kubernetes with autoscaling using HPA.

## Core workflow

- Start from a trained ML model (from ML Zoomcamp) and expose a prediction endpoint using FastAPI, following standard request/response schemas.  
- Package the FastAPI app and model into a Docker image, focusing on a small, reproducible environment and dependency pinning.  
- Push the image to a container registry so Kubernetes can pull it when creating pods.  

## Kubernetes deployment

- Create Kubernetes manifests (Deployment, Service) to run multiple replicas of the FastAPI model service and expose it inside or outside the cluster.  
- Use resource requests/limits so the scheduler can place pods correctly and to enable meaningful autoscaling later.  
- Apply and update manifests iteratively with `kubectl` while observing pod status and logs for debugging.  

## Autoscaling and operations

- Configure a Horizontal Pod Autoscaler (HPA) to scale the number of pods based on CPU or other metrics, demonstrating live scaling under load.  
- Use `kubectl get`/`describe` commands to monitor Deployments, Services, and HPA behavior, and to verify that traffic is being distributed across replicas.  
- Emphasize that this pattern (FastAPI + Docker + K8s + HPA) is a reusable blueprint for production-style ML model deployments.

[1](https://www.youtube.com/live/c_CzCsCnWoU)


## Appendix

deploy tensor flow model, tfserving , using Kind, 

EKS - elastic kubernetes service...

EKSCTL - similar to Kubectl

Create cluster using eksctl ---- eksctl create cluster --name zoomcamp-eks...

#### Deploying to EKS
1. use EKSCTL CLI. Install in bin folder, wget from the website, and use the tar.gz.  Extract the tar , tar xzfv eksctl_Linux_amd64.tar.gz.  Remove the tar.gz

<img width="950" height="309" alt="image" src="https://github.com/user-attachments/assets/a5b34d6b-7e6c-4886-b13f-0d77b4c8f7f3" />
<img width="961" height="137" alt="image" src="https://github.com/user-attachments/assets/be377d0f-0d54-465c-9fae-ef287c9fa165" />


2. Go back to working directory. Issue
   ```bash
      eksctl create cluster --name zoomcamp-eksctl
   ```
3. Create eks-config.yaml
<img width="721" height="442" alt="image" src="https://github.com/user-attachments/assets/ac368455-f27e-4008-ad6a-d4624fba3cbe" />

<img width="1248" height="678" alt="image" src="https://github.com/user-attachments/assets/88dfbb49-3468-4e3f-b9d2-6f3d1ee66a67" />

```bash
   eksctl create cluster -f eks-config.yaml
   aws ecr create-repository --repository-name mlzoomcamp-images
```
4. 
5. 
6. 
7. set-up config.yaml 
8. EKS cluster is **not *free*** ,
9. apply the config,
    ```bash
       kubectl apply -f model-deployment.yaml
       kubectl apply -f model-service.yaml
       kubectl get pod
       kubectl get service
       kubectl port-forward service/tf-serving-clothing-model 8500:8500
       kubectl apply -f gateway-deployment.yaml
       kubectl apply -f gateway-service.yaml
       kubectl get pod
       kubectl get service
       kubectl port-forward service/gateway 8080:80 
    ```
11.    Get URL go to the test file, and put the long URL...
<img width="1264" height="763" alt="image" src="https://github.com/user-attachments/assets/0eeb4b26-7ef8-4821-ae1b-17e6720a98ac" />

12.   type of service is load balancer, and it goes ... deploy model with kubernetes, aws takes care of it..  load balancer is open to everyone.... often times you want to restrict services, you need to do the same thing, lambda function.
13.   Stop cluster
```bash
   eksctl delete cluster --name mlzoomcamp-eks

```
14.   
15.           
16. 
#### Difference between Kubernetes, Kind and Kubectl
Kubernetes, Kind, and Kubectl differ in their function: Kubernetes is the orchestration platform, Kind is a tool used to create local Kubernetes clusters for development/testing, and Kubectl is the command-line interface used to interact with any Kubernetes cluster. 
Here is a breakdown of each component:
Kubernetes (K8s): This is an open-source platform for managing containerized workloads and services at scale. It orchestrates tasks like deployment, scaling, and management of applications in a cluster of machines (nodes). It is the underlying system that manages your applications.
Kind (Kubernetes in Docker): This is a tool designed to run a local Kubernetes cluster by using Docker containers as the cluster nodes. Kind is particularly useful for testing Kubernetes itself, for local development, and for continuous integration (CI/CD) pipelines because it can quickly create and delete multi-node clusters. It requires Docker to run.
Kubectl: This is the command-line interface (CLI) tool for users to interact with a Kubernetes cluster. You use kubectl commands (e.g., kubectl get nodes, kubectl deploy application) to send instructions to the Kubernetes cluster's API server, whether that cluster is local (like one created by Kind) or managed in the cloud (like Google Kubernetes Engine (GKE) or Amazon EKS). 
In summary:
Component 	Type	Primary Function
Kubernetes	Platform	Container orchestration and management system.
Kind	Tool	Creates and runs a local Kubernetes cluster (using Docker).
Kubectl	CLI Tool	Manages and interacts with any running Kubernetes cluster.

#### Workshop steps


1. Install Kubernetes
```bash
cd 

mkdir bin && cd bin

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl

cd

export PATH="${PATH}:${HOME}/bin"  #  add it to .bashrc
                                  

which kubectl
```
   
2. Install Kind (kubernetes in docker)  kubernetes cluster that can be run locally 
```bash
curl -Lo ${HOME}/bin/kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ${HOME}/bin/kind
```
3. Verify Installation
```bash
kind version
```
4. Install uv (package manager for python application)
```bash
pip install uv
```
5. Create uv project
```bash
uv init
```
6. Create a KIND cluster
```bash
kind create cluster --name mlzoomcamp
kubectl cluster-info
kubectl get nodes

```

Create a single-node Kubernetes cluster, configure kubectl to use this cluster, and then take a few minutes on first run.  , you should see Node Ready status.
   
7. Convert model to ONNX (model agnostic environment for inference)
We use pre-trained Pytorch model that classifies clothing items, which is in ONNX format

```bash
mkdir service 
cd service

wget https://github.com/DataTalksClub/machine-learning-zoomcamp/releases/download/dl-models/clothing_classifier_mobilenet_v2_latest.onnx -O clothing-model.onnx
```

8.  Build the FASTAPI service

   ```bash
   ```
   
