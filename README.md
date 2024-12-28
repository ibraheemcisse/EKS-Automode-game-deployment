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

``bash
eksctl create cluster -f cluster-config.yaml

defre
