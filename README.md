# Node.js "Hello World" Application with Docker, Helm, and ArgoCD

This guide will walk you through the steps to Dockerize a Node.js "Hello World" application, create a Kubernetes Helm chart, and deploy it using ArgoCD for GitOps management.

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

    Refer Dockerfile

3. **Build and Test the Docker Image Locally:**

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

    Refer values.yaml

3. **Update `templates/deployment.yaml`:**

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: {{ include "node-hello.fullname" . }}
      labels:
        {{- include "node-hello.labels" . | nindent 4 }}
    spec:
      replicas: {{ .Values.replicaCount }}
      selector:
        matchLabels:
          {{- include "node-hello.selectorLabels" . | nindent 6 }}
      template:
        metadata:
          labels:
            {{- include "node-hello.selectorLabels" . | nindent 8 }}
        spec:
          containers:
            - name: {{ .Chart.Name }}
              image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
              ports:
                - containerPort: 3000
              resources:
                {{- toYaml .Values.resources | nindent 12 }}
    ```

4. **Update `templates/service.yaml`:**

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: {{ include "node-hello.fullname" . }}
      labels:
        {{- include "node-hello.labels" . | nindent 4 }}
    spec:
      type: {{ .Values.service.type }}
      ports:
        - port: {{ .Values.service.port }}
          targetPort: 3000
      selector:
        {{- include "node-hello.selectorLabels" . | nindent 4 }}
    ```

## Step 3: Push the Helm Chart to GitHub

1. **Initialize a New Git Repository and Push the Helm Chart:**

    ```sh
    git init
    git add .
    git commit -m "Initial commit with Helm chart"
    git remote add origin https://github.com/<your-github-username>/node-hello-helm.git
    git push -u origin main
    ```

## Step 4: Deploy Using ArgoCD

1. **Set Up ArgoCD on Your Kubernetes Cluster:**

    Follow the [ArgoCD Getting Started Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/).

2. **Create an `application.yaml` File for ArgoCD:**

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: node-hello
      namespace: argocd
    spec:
      project: default

      source:
        repoURL: 'https://github.com/<your-github-username>/node-hello-helm'
        targetRevision: HEAD
        path: node-hello

      destination:
        server: 'https://kubernetes.default.svc'
        namespace: default

      syncPolicy:
        automated:
          prune: true
          selfHeal: true
    ```

3. **Apply the ArgoCD `application.yaml`:**

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
