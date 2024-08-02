
# Train Booking Application Deployment with AWS ECR & ECS

## Project Overview

This project demonstrates the deployment of a containerized train booking application using Amazon Web Services (AWS). The deployment leverages Amazon Elastic Container Registry (ECR) for storing Docker images and Amazon Elastic Container Service (ECS) for running and managing containers. The setup also includes an Application Load Balancer (ALB) to distribute traffic and ensure high availability.

## Setup and Deployment

### 1. Setting Up Docker on EC2

- **Launch an EC2 Instance**: Start by launching an EC2 instance using the desired AMI, instance type, and security group configurations.

- **Login as Root User**: Gain root access on the EC2 instance.
  ```bash
  sudo -i
  ```

- **Install Docker**:
  ```bash
  yum install docker -y
  systemctl start docker
  ```

- **AWS CLI Configuration**: Configure the AWS CLI with your credentials.
  ```bash
  aws configure
  ```
  You will be prompted to enter your AWS Access Key ID, Secret Access Key, region, and output format.

- **Build the Docker Image**: Create a Dockerfile for your train booking application and build the Docker image.
  ```bash
  docker build -t train-booking-app .
  ```

### 2. Pushing Docker Image to Amazon ECR

To securely store and manage Docker images, we utilized **Amazon Elastic Container Registry (ECR)**. ECR offers seamless integration with AWS services, providing a scalable and secure environment for our Docker images. After setting up Docker on an EC2 instance and building the Docker image for our train booking application, we pushed the image to our ECR repository. This integration allows for easy retrieval and management of images within our AWS infrastructure, enhancing the deployment process.

For more details on pushing images to ECR, please refer to the [AWS ECR Documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html).

### 3. Deploying with Amazon ECS

- **Create an ECS Cluster**:
  - Navigate to the ECS dashboard in the AWS Management Console.
  - Create a new cluster with the appropriate configuration (EC2 or Fargate).

- **Define a Task Definition**:
  - In the ECS dashboard, create a new task definition specifying the Docker image URL from ECR and the required container properties (such as CPU and memory allocation).

- **Create a Service**:
  - Define a service using the created task definition, specifying the desired number of task instances.
  - Attach the service to an existing or new Application Load Balancer (ALB) to manage incoming traffic.

### 4. Accessing the Application

Once the service is running and the load balancer is configured, the application can be accessed via the DNS name provided by the ALB. This setup ensures that the application is highly available and can handle varying loads efficiently.

## Docker Swarm Setup

Docker Swarm is an alternative to AWS ECS for managing container orchestration. Here's a brief overview of how to set up a Docker Swarm cluster and deploy the application:

### 1. Setting Up Docker Swarm

- **Decide on the Number of Nodes**: For this example, we’ll use three nodes: one manager node and two worker nodes.

- **Install Docker on All Nodes**:
  - **Login as Root User** on each instance.
    ```bash
    sudo -i
    ```
  - **Install Docker**:
    ```bash
    yum install docker -y
    systemctl start docker
    ```

- **Set Hostnames**:
  - On the manager node, set the hostname to `manager`.
    ```bash
    hostnamectl set-hostname manager
    ```
  - On the worker nodes, set the hostnames to `worker1` and `worker2`.
    ```bash
    hostnamectl set-hostname worker1
    hostnamectl set-hostname worker2
    ```

- **Initialize Docker Swarm on the Manager Node**:
  ```bash
  docker swarm init
  ```
  - Save the token provided, as it will be used to join worker nodes to the swarm.



### 2. Deploying the Application on Docker Swarm

- **Create a Docker Service**:
  - On the manager node, deploy the train booking application with the following command:
    ```bash
    docker service create --name train --replicas 6 --publish 82:80 train-booking-app:latest
    ```
  - This command will create a service named `train` with 6 replicas, exposing port 82 on the host and mapping it to port 80 in the container.

- **Access the Application**:
  - The application can be accessed via the public IP address of any of the manager or worker nodes on port 82.

## Architecture

The deployment architecture includes the following components:

- **Dockerized Application**: The application is containerized using Docker.
- **Amazon ECR**: Stores the Docker image securely.
- **Amazon ECS**: Manages the deployment and scaling of the Docker containers.
- **EC2 Instances or Fargate**: Hosts the Docker containers, depending on the ECS launch type.
- **Application Load Balancer**: Distributes traffic across multiple instances, ensuring high availability.

## Comparison with Other Technologies

### Amazon ECR vs. Docker Hub

- **Security**: Amazon ECR offers tighter security integrations with AWS IAM, enabling fine-grained access control.
- **Integration**: ECR integrates seamlessly with other AWS services, making it ideal for AWS-centric workflows.
- **Performance**: Optimized for the AWS environment, ECR provides faster access times and lower latency compared to Docker Hub.

### Amazon ECS vs. Docker Swarm

- **Load Balancing**: ECS has built-in support for integrating with AWS load balancers, simplifying the process of handling traffic distribution.
- **Scaling and Management**: ECS offers more advanced scaling and management capabilities, including automated scaling based on resource utilization.
- **Ecosystem Integration**: ECS integrates with AWS services like CloudWatch for monitoring, IAM for security, and others, providing a comprehensive cloud-native solution.

## Conclusion

This project provided hands-on experience with deploying a scalable and highly available application using AWS ECR and ECS, as well as setting up Docker Swarm for container orchestration. The integration of these services demonstrates the power of cloud-native architectures and containerization technologies in modern software deployment, offering robust solutions for application management and scaling.
