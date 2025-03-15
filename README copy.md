# Jenkins Pipeline for Application Deployment on Kubernetes 🚀

## 1. Overview 🌍
This project automates the process of building a Docker image, pushing it to DockerHub, and deploying it to a Kubernetes cluster using Jenkins.
**Pipline workflow:**
- Clone a GitHub repository containing application code, a Dockerfile, and a Kubernetes deployment configuration.
- Build a Docker image based on the application.
- Push the image to a DockerHub repository.
- Deploy the application to a Kubernetes cluster using a deployment configuration file.

This setup ensures seamless CI/CD integration, allowing for automated deployments with minimal manual intervention.

---

## 2. Prerequisites 📝

### Kubernetes Cluster 🖥️
- A running Kubernetes cluster Minikube
```bash
minikube start
minikube status
```

### installation of jenkins as a service 
```bash
 brew install jenkins-lts
 brew services start jenkins-lts
 ```

### Jenkins Setup ⚙️
- A Jenkins server with the required plugins installed:
  - **Kubernetes Plugin**
  - **Docker Pipeline Plugin**
  - **Git Plugin**

### Required Credentials 🔑
- **GitHub Credentials**: To access the GitHub repository.
- **DockerHub Credentials**: To push Docker images.
- **Kubernetes ServiceAccount Token**: To authenticate with the Kubernetes cluster.

---

## 3. Steps 

### Step 1: Create a Kubernetes ServiceAccount 🆔
- A dedicated **ServiceAccount** is created in Kubernetes to allow Jenkins to authenticate and interact with the cluster.

### Step 2: Configure RBAC for the ServiceAccount 🔐
- **Role-Based Access Control (RBAC)** is configured to grant permissions for Kubernetes resources such as:
  - Pods
  - Deployments
  - Services
  - ConfigMaps
  - Secrets
  - Jobs
- The **RoleBinding** associates the ServiceAccount with the necessary permissions.
- all this in **service-account.yaml**
```bash
kubectl apply -f service-account.yaml
```

### Step 3: Get the Service Account Token:⚙️
```bash
kubectl get secret $(kubectl get serviceaccount jenkins-service-account -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode
```
or
```bash
kubectl create token jenkins -n jenkins --duration=8760h
```
- Add a new credential of type Secret text and paste the service account token.

 📸![Alt text](assets/pic1.png)

### Step 4: Create the Jenkins Pipeline 📝
- Define a **Jenkinsfile** to automate the CI/CD process:
  - Clone the GitHub repository.
  - Build the Docker image.
  - Push the image to DockerHub.
  - Update the deployment configuration.
  - Deploy the application to Kubernetes.
- The pipeline is structured to handle success and failure scenarios efficiently.

 📸![Alt text](assets/pic2.png)

### Step 5: Test the Deployment 🏗️
- Verify the Kubernetes deployment status by checking:
  - Active **deployments**
  - Running **pods**

 📸![Alt text](assets/pic3.png)

- Docker Image Creation

 📸![Alt text](assets/pic4.png)



