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

This project provided hands-on experience with deploying a scalable and highly available application using AWS ECR and ECS. The integration of these services demonstrates the power of cloud-native architectures in modern software deployment, offering robust solutions for application management and scaling.

