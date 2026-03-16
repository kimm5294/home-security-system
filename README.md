# Home Security System

Individual Project Two using Docker, Kubernetes, CI/CD and microservices. This repository contains three services listed below. Although they are in the same repository, they are still separate services. 

## Services
- **home-sensor-listener**
- **home-sensor-processor**
- **home-sensor-alert-service**

## AWS Services Used:
- Elastic Kubernetes Service
- Elastic Container Registry
- AWS App Mesh
- Simple Queue Service
- Simple Notification Service
- Relational Database Service

## Problem Statement
Homeowners cannot monitor their homes at all times. This system receives sensor events, processes and stores them, and sends real-time email alerts to the homeowner.

## Setup
This service was setup by creating the three main services, containerizing them and adding them to ECR. These docker images are hosted in pods and managed by EKS. The listener and alert service pod have Envoy sidecars with them that can help manage traffic and improve observability. This is part of AWS Mesh App. The listener service receives requests via HTTP but then communicates with the processor using SQS queues. The processor communicates with RDS and SQS. There is no Envoy sidecar with the processor due to some complications with RDS communication but it is possible to add later on. The alert service gets messages from the processor via SQS then sends messages to SNS, which will send the user an email. 

## Deploy
Pushing changes to master branch triggers the CI/CD pipeline using Github Actions. This will deploy any changes to EKS including new docker images. 
