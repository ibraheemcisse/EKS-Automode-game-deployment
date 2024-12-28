# Deploy a 2048 Game Application on Amazon EKS

This project demonstrates how to deploy a **2048 game application** on **Amazon EKS** (Elastic Kubernetes Service), with **persistent data storage** on **EBS** (Elastic Block Store) volumes. It leverages **EKS Auto Mode** to automate various aspects of cluster management, including node provisioning, load balancing, and dynamic scaling of Kubernetes workloads.

The application uses Kubernetes resources such as **Deployments**, **Services**, **Ingress**, and **Persistent Volume Claims (PVCs)** to ensure the persistence of game data and provide external access to the game.

## Project Overview

This project will guide you through the process of:

1. Creating an EKS cluster using **eksctl** with **Auto Mode** configuration.
2. Setting up an EBS-backed Persistent Volume to store game data.
3. Deploying the 2048 game application using Kubernetes **Deployment**.
4. Exposing the game to the internet via an **Ingress** and an **Application Load Balancer (ALB)**.
5. Managing dynamic scaling and handling failure recovery automatically with EKS Auto Mode.

## Prerequisites

Before you start, ensure you have the following:

- **AWS CLI** installed and configured with proper IAM permissions.
- **eksctl** installed for creating and managing EKS clusters.
- **kubectl** installed to interact with Kubernetes clusters.
- A basic understanding of Kubernetes concepts such as Deployments, Services, and Ingress.

## Setup Instructions

### 1. Create the EKS Cluster

Create an EKS cluster using the configuration file `cluster-config.yaml`. The cluster will be set up in **Auto Mode** to handle scaling of EC2 instances and will automatically configure networking, load balancing, and storage.

Run the following command:

**eksctl create cluster -f cluster-config.yaml**

This will create a VPC, subnets, and the necessary EKS control plane components. You can monitor the cluster creation progress using:

**eksctl utils describe-stacks --region us-east-1 --cluster web-quickstart**

### 2. Set Up Persistent Storage (EBS)

To store game data, you'll need to create a Persistent Volume Claim (PVC) backed by EBS. Use the ebs-pvc.yaml file to define the PVC:

**kubectl apply -f ebs-pvc.yaml**

Additionally, you should define a StorageClass that will provision EBS volumes. The storage-class.yaml file defines a default storage class using gp3 volumes, which are both performant and cost-effective.

**kubectl apply -f storage-class.yaml**

3. Deploy the 2048 Game Application

The next step is to deploy the 2048 game application. The game’s deployment configuration is defined in the deployment.yaml file.

Run the following command to deploy the game:

**kubectl apply -f deployment.yaml**

This deployment will create 3 replicas of the 2048 game application, which will be managed by Kubernetes.

4. Expose the Application via an Ingress
   
To expose the game externally, we use an Ingress resource along with an Application Load Balancer (ALB). The ingress-2048.yaml file defines an Ingress with the necessary annotations for ALB integration.


**kubectl apply -f ingress-2048.yaml**

The Ingress will automatically route traffic from the ALB to the Kubernetes service for the 2048 application.

5. Access the Game
   
After the Ingress is set up, you can access the game via the ALB’s DNS name. To find the ALB URL, run:

**kubectl get ingress -n game-2048**

It should show the DNS name for the ALB, which you can use to access the game in your browser.

6. Scaling the Application (Optional)

EKS Auto Mode automatically scales nodes, but you can also scale the application’s replicas manually by adjusting the replicas field in deployment.yaml and applying the changes:

**kubectl apply -f deployment.yaml**

7. Clean Up Resources
   
Once you're done, you can delete the cluster and associated resources with:

**eksctl delete cluster -f cluster-config.yaml**

This will remove all resources, including the EKS cluster, VPC, and associated components.

**Files in the Repository**

cluster-config.yaml: EKS cluster configuration file used with eksctl to create an EKS Auto Mode cluster.
deployment.yaml: Kubernetes deployment file for the 2048 game application.
ebs-pvc.yaml: Persistent Volume Claim (PVC) for storing game data on EBS.
storage-class.yaml: Kubernetes StorageClass definition for dynamically provisioning EBS volumes.
ingress-2048.yaml: Ingress definition with annotations for integrating with AWS ALB.
service.yaml: Service definition to expose the 2048 game application internally in the cluster.

