# Scalable Image Annotation Application

A robust, cloud-native AWS solution combining an EC2-hosted web interface with serverless image processing. Users can upload images, view a gallery of AI-generated annotations and thumbnails, showcasing expertise in AWS architecture, Infrastructure as Code, and generative AI integration.

## Overview

This project demonstrates:
- **Hybrid Architecture**: A Python/Flask web frontend on EC2 Auto Scaling Group (ASG) behind an Application Load Balancer (ALB).
- **Serverless Processing**: AWS Lambda functions triggered by S3 events to generate image captions via the Gemini API and to create thumbnails.
- **Secure Storage**: Images and thumbnails in Amazon S3; metadata in Amazon RDS (MySQL).
- **Event-Driven Integration**: S3 notifications fan out through Amazon SNS to Lambda subscribers.

## Features

- **Image Upload & Listing**  
  Users can upload images via an HTML form; uploads are stored in S3 and metadata saved to RDS.  
- **AI-Powered Annotations**  
  A Lambda function invokes the Gemini API to generate descriptions, storing results in RDS.  
- **Automated Thumbnails**  
  A second Lambda function generates and stores thumbnails in S3 under `thumbnails/`.  
- **Auto Scaling & Load Balancing**  
  EC2 instances scale out/in based on CPU utilization, with traffic distributed by an ALB.  
- **Infrastructure as Code**  
  Complete deployment via AWS CloudFormation.

## Architecture

### Web Application Architecture

![Web App Architecture](docs/web_app_architecture.png)

### Serverless Architecture

![Serverless Architecture](docs/serverless_architecture.png)

## AWS Services Used

- Amazon VPC (custom subnets, NAT Gateway)  
- Amazon EC2 (Launch Template, Auto Scaling Group)  
- Elastic Load Balancing v2 (Application Load Balancer)  
- Amazon S3 (image & thumbnail storage)  
- Amazon RDS for MySQL (metadata storage)  
- AWS Lambda (annotation & thumbnail functions)  
- Amazon SNS (S3 event notifications)  
- AWS CloudFormation (<a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html">infrastructure as code</a>)  
- AWS IAM (Roles, Policies, Instance Profiles)

## Prerequisites

- AWS account with permissions to deploy CloudFormation stacks  
- AWS CLI configured (<a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html">installation guide</a>)  
- Git installed locally  
- A Gemini API key for image annotation

## Deployment

1. **Clone the repo**  
   ```bash
   git clone https://github.com/your-username/scalable-image-annotation-app.git
   cd scalable-image-annotation-app
   ```
2. **Customize parameters**
   Edit infrastructure/template.yaml to configure:

- KeyName (your EC2 SSH key pair)
- MyIP (your CIDR block for SSH)
- LambdaRoleArn (IAM role ARN for the Lambda functions)
- GEMINI_API_KEY (your Gemini API key)
   
3. **Deploy with CloudFormation**
   ```bash
   aws cloudformation deploy \
   --template-file infrastructure/template.yaml \
   --stack-name image-annotation-app \
   --capabilities CAPABILITY_NAMED_IAM
   ```

4. **Retrieve the URL**

   After deployment completes, note the LoadBalancerDNSName output from the CloudFormation stack. Open that address in your browser to access the live application.
