# Spring Petclinic on AWS EKS

This repository contains the **Spring Petclinic** application deployed on **AWS Elastic Kubernetes Service (EKS)**.

---

## Project Overview
This project demonstrates deploying a **Spring Boot** application with Kubernetes on **AWS EKS**, including containerization, cluster setup, deployment, and public access via Load Balancer.

The workflow covers:
1. Containerizing the Spring application and pushing it to **Amazon ECR**.
2. Creating a managed Kubernetes cluster using **EKS**.
3. Deploying the application using Kubernetes manifests.
4. Exposing the application via a LoadBalancer service.
5. Verifying public access through a web browser.

---

## Deployment Commands and Explanation

### 1. Create an EKS Cluster
```bash
eksctl create cluster \
  --name spring-cluster \
  --version 1.32 \
  --region eu-north-1 \
  --nodes 1 \
  --node-type t3.small
````

**Explanation:**

* `eksctl create cluster`: Creates a new EKS cluster.
* `--name spring-cluster`: Sets the cluster name.
* `--version 1.32`: Kubernetes version.
* `--region eu-north-1`: AWS region for deployment.
* `--nodes 1`: Number of worker nodes.
* `--node-type t3.small`: EC2 instance type for each node.
* **Purpose:** Provisions a managed Kubernetes cluster on AWS with specified nodes.

---

### 2. Apply Kubernetes Manifests

```bash
kubectl apply -f eks/eks-spring.yaml
```

**Explanation:**

* Deploys the Spring application pods, services, and config maps defined in `eks-spring.yaml`.

---

### 3. Check Kubernetes Resources

```bash
kubectl get all
```

**Explanation:**

* Lists all pods, services, deployments, and replica sets in the current namespace.
* Verifies the application is running and the LoadBalancer service has an external IP.

---

### 4. Access the Application

```bash
kubectl get svc spring-service
```

* Use the `EXTERNAL-IP` or DNS to access the application in a web browser:

```
http://<EXTERNAL-IP>/owners/find
```

* Confirms the app is publicly accessible via the AWS Load Balancer.

---

### 5. Optional: Delete the Cluster

```bash
eksctl delete cluster --name spring-cluster
```

* Deletes the EKS cluster and all resources to avoid ongoing AWS costs.

---

## Deployment Steps and Components

### 1. Amazon ECR Repository

* The Spring application's Docker image is built and pushed to **Amazon ECR** (`spring-eks`).
* **Purpose:** Makes the image available for the EKS cluster.

### 2. EKS Cluster Creation

* Shown with `eksctl create cluster` command.
* **Purpose:** Creates the managed Kubernetes infrastructure.

### 3. Cluster Status on AWS

* The `spring-cluster` is **Active** running Kubernetes version 1.32.
* **Purpose:** Confirms successful creation of the cluster.

### 4. Kubernetes Resources

* **Pods:** 3 running instances of the application.
* **Service:** `spring-service` of type LoadBalancer.
* **External-IP:** DNS of AWS ELB assigned to service.
* **Deployment & ReplicaSet:** 3 replicas running.
* **Purpose:** Shows the application is running and exposed externally.

### 5. AWS Load Balancer

* Automatically provisioned for the LoadBalancer service.
* Confirms AWS infrastructure is managing traffic to pods.

### 6. Application Access

* URL example: `http://a8bb7e75d1b77456e89758593f740e8f-35119968.eu-north-1.elb.amazonaws.com/owners/find`
* Confirms app is publicly accessible.

---

## Screenshots

1. **ECR Repository:** `ECR.png`
2. **Cluster Creation Command:** `create-cluster.png`
3. **EKS Console:** `Cluster-on-aws.png`
4. **Kubernetes Resources:** `all.png`
5. **AWS Load Balancer:** `loadbalancer.png`
6. **Application Access:** `spring.png`

---

## Summary

This project demonstrates an **end-to-end cloud-native deployment** workflow:

1. Containerize Spring Boot application → push to ECR.
2. Create EKS cluster with `eksctl`.
3. Deploy application on Kubernetes.
4. Expose using a LoadBalancer service in AWS.
5. Verify the application is accessible via web browser.

```
