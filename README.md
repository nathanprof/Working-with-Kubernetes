# Kubernetes on the go
How to use Kubernetes in a lab environment using a minikube cluster on a VMware workstation running Ubuntu 23.04.
# Installation Steps in VMware Workstation Running Ubuntu Linux
- sudo apt update && sudo apt upgrade -y (This ensures the ubuntu linux machine is up-to-date, where what the update cmd does is updates the package list while the upgrade cmd installs the the package lists in the system.)
## install virtualbox extension
- sudo apt install virtualbox virtualbox-dkms virtualbox-qt virtualbox-ext-pack
## Install Docker 
- Before running thois cmd make sure to be root e.g sudo su
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
sudo apt update
## Install Minikube
- wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 chmod +x minikube-linux-amd64 sudo mv minikube-linux-amd64 /usr/local/bin/minikube
- minikube version
## Install Kubectl
- sudo apt install curl (in case you don't have curl installed on your device)
- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
- chmod +x ./kubectl (To make the kubectl executable)
- sudo mv ./kubectl /usr/local/bin/kubectl (this cmd ensures that from any location in your ubuntu linux location on your terminal you can just type kubectl and it it will work)
- kubectl version (cmd to verify the version of the kubectl downloaded)
  # Creating PODs in Kubernetes
  - mkdir reggiekubpods (cmd to create a directory  named reggiekubpods)
  - cd reggiekubpods (navigate into the directory)
  - minikube start --driver=docker (start a Minikube cluster using the Docker driver)
  - kubectl run elinathanpod --image=redis (create a new pod elinathanpod)
  - kubectl get pods (list the pods that were created)
  - kubectl logs elinathanpod (retrieve the logs of the pod)
  - kubectl run elinathan1pod --image=nginx
  - kubectl port-forward elinathan1pod 8000:80 (forward localport 8000 to port 80)
  - curl http://localhost:8000 (to verify that the port 8000 is open and working)
  - kubectl get pod elinathanpod -o yaml (display  podin yaml format) 
  - kubectl describe pod elinathanpod (show details of a pod in this case)
  - kubectl delete pods --all (delete all pods that were created)
  - kubectl run reggiepod --dry-run=client --image=nginx -o yaml > elinathan.yaml (prints the configuration of a pod without actually creating it to the file elinathan.yaml)
  - cat elinathan.yaml (prints the contents of elinathan.yaml)
  - vim elinathan.yaml (use vim to edit the file and add an additional image with a unique name a.k.a as a side car)
  - kubectl apply -f elinathan.yaml (create or update the file elinathan.yaml)
  # Deploying PODS
  - kubectl create deployment elinathandep --image=redis (create new resource)
  - kubectl get deployment (list the created deployment)
  - kubectl edit deployment elinathandep (open the yaml manifest ina text editor)
  - kubectl get deployment (list the information about the deployment)
  - kubectl delete deployment elinathandep (deletes the deployment)
  - kubectl expose  deployment reggiedep --type=NodePort --port=8000 (takes the deployment and exposes it to another kubernetes service)
  - kubectl get services (displays the services)
  - kubectl describe deployment reggiedep (show details of the deployment)
  - kubectl describe service reggiedep (show details of the service)
  - kubectl scale deployment nathan-dep --replicas=12 (sets a new size for deployment)
  - minikube addons enable metrics-server (enables the metrics server addons)
  - minikube dashboard (access the kubernetes running within the minikube cluster)
  - kubectl delete service reggiedep
  - minikube stop
