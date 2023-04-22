# Install VirtualBox and Ubuntu on it

We will use this virtual machine for the next Continuous Delivery part of this project.

# Setup Minikube

Minikube is a tool that enables users to set up and run a single-node Kubernetes cluster on their local machine. It is useful for developers who want to test their applications in a local Kubernetes environment before deploying them to a production cluster. Minikube provides an easy way to learn and experiment with Kubernetes without the need for a complex setup.

After performing the steps we install Minikube and start it.
```
sudo apt-get update
sudo apt-get install docker.io
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo usermod -aG docker $USER && newgrp docker
sudo reboot docker
minikube start --driver=docker
```
# Install kubectl

Kubectl is a command-line interface (CLI) tool that is used to interact with Kubernetes clusters. It allows users to deploy, inspect, and manage Kubernetes resources such as pods, deployments, services, and more. Kubectl enables users to perform operations such as creating, updating, deleting, and scaling Kubernetes resources.

Run the following steps to install kubectl.
```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version
```
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_1.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_2.avif)

# Install Argo CD operator
ArgoCD is a widely-used GitOps continuous delivery tool that automates application deployment and management on Kubernetes clusters, leveraging Git repositories as the source of truth. It offers a web-based UI and a CLI for managing deployments, and it integrates with other tools. ArgoCD streamlines the deployment process on Kubernetes clusters and is a popular tool in the Kubernetes ecosystem.

The Argo CD Operator manages the full lifecycle of Argo CD and its components. The operator's goal is to automate the tasks required when operating an Argo CD cluster.
```
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.24.0/install.sh | bash -s v0.24.0
kubectl create -f https://operatorhub.io/install/argocd-operator.yaml
kubectl get csv -n operators
kubectl get pods -n operators
```
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_3.avif)

Goto link https://argocd-operator.readthedocs.io/en/latest/usage/basics/

The following example shows the most minimal valid manifest to create a new Argo CD cluster with the default configuration.

Create argocd-basic.yml with the following content.
```
apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: example-argocd
  labels:
    example: basic
spec: {}
```
```
kubectl apply -f argocd-basic.yml
kubectl get pods
kubectl get svc
kubectl edit svc example-argocd-server
minikube service example-argocd-server
kubectl get secret
```
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_4.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_5.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_6.avif)
NodePort services are useful for exposing pods to external traffic where clients have network access to the Kubernetes nodes.

```kubectl edit svc example-argocd-server``` And change from ClusterIP to NodePort. Save it.
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_1.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_7.avif)

# Password for Argo CD

Find out password for Argo CD, so that, we can access Argo CD web interface.
```
kubectl get secret
kubectl edit secret example-argocd-cluster
```
Copy admin.password

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_8.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_2.avif)
```
echo <admin.password> | base64 -d
```
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_3.avif)

# Argo CD Configuration

Username : admin

Password : 2zjrcTKFRvEgBZW1ftUO4GuodQD5A9CH

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_4.avif)

We will use the Argo CD web interface to run sprint-boot-app.

Setup Github Repository manifest and Kubernetes cluster.

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_5.avif)
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/argocd_6.avif)

After Create. You can check if pods are running for sprint-boot-app

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/minikube_9.avif)

You have now successfully deployed an application using Argo CD.

Argo CD is a Kubernetes controller, responsible for continuously monitoring all running applications and comparing their live state to the desired state specified in the Git repository.

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/Screenshot%20from%202023-04-21%2017-00-21.png)

To check if our service is working, execute the command:
```
kubectl get svc
```
![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/kubectl-get-svc.png)

Now we need to get the node IP with 
```
kubectl get nodes -o wide 
```
so that we can access our app.

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/kubectl-get-wide.png)

Final result.

![Minikube](https://github.com/Edgar85/Jenkins-Zero-To-Hero/blob/main/Screenshots/Screenshot%20from%202023-04-21%2017-00-54.png)
