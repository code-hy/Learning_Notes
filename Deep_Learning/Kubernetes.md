# Workshop Notes

This video walks through taking an existing ML model, wrapping it in a FastAPI web service, containerizing it with Docker, and deploying it on Kubernetes with Horizontal Pod Autoscaling so it can scale under load. It uses example code from the ML Zoomcamp repository and a dedicated workshop repo to show a practical, production-style deployment pattern.  

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
