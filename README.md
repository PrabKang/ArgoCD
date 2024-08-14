# Installing latest/stable version of ArgoCD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Forward Ports
```
kubectl get services -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:443
```

### Get Credentials
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### Kubectl Dev Enviroment
```
kubectl apply -f appplication.yaml
```

### Kubectl Prod Enviroment
```
kubectl apply -f appplication-prod.yaml
```

# Install ArgoCD CLI / Login via CLI (MacOS)
```
brew install argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443
argocd login 127.0.0.1:8080
```
# Deployment YAML Explanation

* apiVersion: Defines the version of the Kubernetes API you're using for the deployment resource. apps/v1 is the stable version for Deployments.
* kind: Specifies the type of Kubernetes resource you're defining. In this case, it's a Deployment.
* metadata: Includes metadata about the deployment, such as its name.
* spec: Defines the desired state of the deployment, including how many replicas (Pods) you want and how the Pods should be configured.
selector: Matches Pods based on their labels. This ensures that the deployment knows which Pods it should manage.
* replicas: Specifies the number of identical Pods that should be running.
* template: Describes the Pods that will be created. It includes both metadata (like labels) and specifications for the containers inside the Pods.
* containers: Lists the containers that should be running in each Pod. Each container has a name, an image, and can expose ports, among other settings.

# Service YAML Explanation

* apiVersion: Defines the version of the Kubernetes API you're using for the service resource. v1 is the stable version for core API objects like Services.
* kind: Specifies the type of Kubernetes resource you're defining. In this case, it's a Service.
* metadata: Includes metadata about the service, such as its name.
* spec: Defines the desired state and configuration of the service, including which Pods it should route traffic to and how it should expose that traffic.
* selector: Matches Pods based on their labels. This tells the service which Pods to target and send traffic to.
* ports: Defines the port configuration for the service.
    * port: The port that will be exposed by the service and accessible externally.
    * protocol: The protocol used for communication (usually TCP or UDP).
    * targetPort: The port on the Pod that the service will forward traffic to. This is typically the port the application inside the container listens on.

# Application YAML Explanation

* apiVersion: Specifies the version of the ArgoCD API you're using for the Application resource.
* kind: Defines the type of resource, which in this case is an Application.
* metadata: Contains metadata like the name and namespace of the application resource.
* spec: Specifies the desired state and configuration of the application, including the source of the application manifests, the destination where they should be deployed, and the sync policy.
* source: Defines where the application's manifests or Helm chart are located, including the Git repository URL, the branch or commit to use, and the path within the repository.
destination: Specifies the Kubernetes cluster and namespace where the application should be deployed.
* syncPolicy: Describes the synchronization behavior, including options for automatically creating namespaces, self-healing, and pruning untracked resources.