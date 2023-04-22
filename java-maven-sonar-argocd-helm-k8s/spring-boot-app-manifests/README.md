Install VirtualBox and Ubuntu on it

We will use this virtual machine for the next Continuous Delivery part of this project.

Setup Minikube

Minikube is a tool that enables users to set up and run a single-node Kubernetes cluster on their local machine. It is useful for developers who want to test their applications in a local Kubernetes environment before deploying them to a production cluster. Minikube provides an easy way to learn and experiment with Kubernetes without the need for a complex setup.

After performing the steps we install Minikube and start it.

sudo apt-get update
sudo apt-get install docker.io
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo usermod -aG docker $USER && newgrp docker
sudo reboot docker
minikube start --driver=docker

Install kubectl

Kubectl is a command-line interface (CLI) tool that is used to interact with Kubernetes clusters. It allows users to deploy, inspect, and manage Kubernetes resources such as pods, deployments, services, and more. Kubectl enables users to perform operations such as creating, updating, deleting, and scaling Kubernetes resources.

Run the following steps to install kubectl.

curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version

