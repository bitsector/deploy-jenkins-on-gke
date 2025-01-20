# deploy-jenkins-on-gke

[Full Article](https://bitsector.notion.site/Deploying-Jenkins-on-Google-Kubernetes-Engine-GKE-180cfe2b2f5c805bb5f0fce93ed5cd8a)

### Create a node pool specific for Jenkins
```bash
gcloud container node-pools create devops-pool \
  --cluster=$CLUSTER_NAME \
  --machine-type=n1-standard-4 \
  --num-nodes=1 \
  --zone=$ZONE \
  --project=$PROJECT_NAME
```
### Get the node name
```bash
kubectl get nodes \
  -l cloud.google.com/gke-nodepool=devops-pool \
  | grep -v NAME \
  | cut -d' ' -f1
```

### Label the node with the "jenkins-node" label
```bash
kubectl label nodes <node-name> jenkins-node=true
```

### Create "devops-tool-suite" namespace
```bash
kubectl create namespace devops-tool-suite
```

### Apply the service account configuration
```bash
kubectl apply -f service-account.yaml
```

### Apply the volume configuration
```bash
kubectl apply -f volume.yaml
```

### Apply the deployment configuration
```bash
kubectl apply -f deployment.yaml
```

### Port forward to local host and connect to the Jenkins controller
```bash
kubectl port-forward deployment/deployment-jenkins 9090:8080 -n devops-tool-suite
```

### Open the web UI in the browser (Linux specific)
```bash
google-chrome http://localhost:9090
```
