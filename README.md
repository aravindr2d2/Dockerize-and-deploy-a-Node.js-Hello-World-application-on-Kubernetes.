# Node.js "Hello World" Application with Docker, Helm, and ArgoCD

This guide will walk you through the steps to Dockerize a Node.js "Hello World" application, create a Kubernetes Helm chart, and deploy it using ArgoCD.

## Prerequisites

- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/) cluster
- [Helm](https://helm.sh/)
- [ArgoCD](https://argoproj.github.io/argo-cd/)
- [GitHub](https://github.com/) account
- [DockerHub](https://hub.docker.com/) account

## Step 1: Dockerize the Application

1. **Clone the Repository:**

    ```sh
    git clone https://github.com/johnpapa/node-hello
    cd node-hello
    ```

2. **Create a Dockerfile:**

    Refer [Dockerfile](Dockerfile)
    

4. **Build and Test the Docker Image Locally:**

    ```sh
    docker build -t node-hello .
    docker run -p 3000:3000 node-hello
    ```

## Step 2: Create a Helm Chart

1. **Create a New Helm Chart:**

    ```sh
    helm create node-hello
    ```

2. **Update `values.yaml`:**

    Refer [values.yaml](values.yaml)

3. **Update `templates/deployment.yaml`:**

    Refer [deployment.yaml](deployment.yaml)

4. **Update `templates/service.yaml`:**

   Refer [service.yaml](service.yaml)
    
## Step 3: Push the Helm Chart to GitHub

1. **Initialize a New Git Repository and Push the Helm Chart:**

    ```sh
    git init
    git add .
    git commit -m "Initial commit with Helm chart"
    git remote add origin https://github.com/aravindr2d2/Dockerize-and-deploy-a-Node.js-Hello-World-application-on-Kubernetes.git
    git push -u origin main
    ```

## Step 4: Deploy Using ArgoCD

1. **Set Up ArgoCD on Your Kubernetes Cluster:**

    Follow the [ArgoCD Getting Started Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/).

2. **Create an `application.yaml` File for ArgoCD:**

   Refer [application.yaml](application.yaml)

4. **Apply the ArgoCD `application.yaml`:**

    ```sh
    kubectl apply -f application.yaml
    ```

ArgoCD will now monitor the Git repository for changes and automatically deploy the Helm chart to your Kubernetes cluster.

## Summary

By following these steps, you have successfully Dockerized the Node.js "Hello World" application, created a Helm chart, and deployed it using ArgoCD for GitOps management. You can now manage your application deployment via GitOps with ArgoCD, ensuring that your Kubernetes deployments are consistent and version-controlled.

---

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Helm Documentation](https://helm.sh/docs/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/)
- [GitHub](https://github.com/)
- [DockerHub](https://hub.docker.com/)

---

*Feel free to contribute to this repository by opening issues or submitting pull requests.*
