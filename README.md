# COMP3104 Lab 11 – Containerization

**Student:** Yash Zinzuvadiya (101475983)

Fork of [pritamworld/DevOps_Kubernetes_exercise9](https://github.com/pritamworld/DevOps_Kubernetes_exercise9).

| Folder | What it holds |
| --- | --- |
| [`lab11-Docker/`](lab11-Docker) | Part I: a Flask app, its `requirements.txt` and the `Dockerfile` that builds the `hellodocker` image |
| [`k8/`](k8) | Part II: the nginx Deployment (3 replicas) and NodePort Service manifests for Minikube |
| [`screenshots/`](screenshots) | Terminal screenshots of every step |
| `Week11_Lab_Exercise_101475983.docx` | The lab report: each step's description, commands and screenshot |

## Part I – Build and run our own Docker image

```bash
cd lab11-Docker
docker run hello-world
docker build --tag=hellodocker .
docker image ls
docker run -p 4000:80 hellodocker          # leave running in one terminal
curl http://localhost:4000                 # from a second terminal
docker container ls
docker container stop CONTAINER_ID
```

`curl` returns `Hello Yash!`, the container's hostname (its container ID) and
`cannot connect to Redis, counter disabled`, because no Redis service runs yet.

## Part II – nginx on Kubernetes (Minikube)

```bash
cd k8
minikube start
kubectl apply -f nginx-deploy.yaml
kubectl apply -f nginx-service.yaml
kubectl rollout status deployment/nginx
kubectl get pods -l app=nginx -o wide
kubectl get svc nginx-nodeport

# Access: opens a tunnel and prints a URL (keep that terminal open with the Docker driver on macOS)
minikube service nginx-nodeport --url

# Or hit the NodePort directly
MINIKUBE_IP=$(minikube ip)
curl http://$MINIKUBE_IP:30080

# Cleanup
kubectl delete -f nginx-deploy.yaml
kubectl delete -f nginx-service.yaml
```

With Minikube's Docker driver on macOS, the node IP (`192.168.49.2`) can't be
reached from the host, so a direct `curl` to the NodePort times out. The
NodePort does answer from inside the node (`minikube ssh -- curl -s http://$(minikube ip):30080`),
and `minikube service --url` reaches it through a tunnel.
