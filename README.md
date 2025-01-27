# DevOps Challenge - Week 2 - Day 1: Dockerized Sports Data



## Project Overview:

In this project, I deployed a docker container using the AWS ECS and fetch sport data from serpapi.com. The container was deployed with aws severless infrastructure using Fargate
## Prerequisites

- serpapi.com API Keys
- AWS Account
- IAM Role/Permissions: Ensure the user or role running the script has the following permissions

## Key components include:

1. Amazon CLI
2. AWS Fargate
3. Amazon ECS: Container services that allows containerization.
4. Amazon ECR: For storing docker images

## STEPS TO FOLLOW:

## Step 1: Clone Github Repository

1. `git clone https://github.com/iamDayoDev/dockerized-sport-data-api`

2. change directory into the cloned directory

## Step 2: Configure AWS CLI

1. In the CLI (Command Line Interface), type
```bash
aws configure
```
2. Follow the prompt and input your Access key and Secret key

## Step 3: Create a repositry in the AWS ECR
1. In the CLI (Command Line Interface), type
```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```
2. Copy you AWS account ID and paste in the following commands to login your account and build the Dockerfile
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com
```
3. To Build the docker file run
```bash
docker build --platform linux/amd64 -t sports-api .
```

4. Tag the docker image
```bash
docker tag sports-api:latest <AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```
5. To push image to the registryadd
```bash
docker push <AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```

6. Go back to go your console and confirm your image build

## Step 4: Create a cluster
1. In the Console, Naviagte to ECS and create a cluster

2. After creating the cluster, Now go to task definition and create a task.
    - Put in name tags where necessary and input your image URI
    - Put in reqiured port, which in this case is port 8080
    - Set your environment variables with proper name in the app.py file and the API Key from serpapi.com

-The task has been successfully created.

## Step 5: Create a Service

1. In the cluster page, navigate to service and create a service.
    - select your task as the family name
    - Input your service name.
    - Select service type as replica and put 2 in the desired replica
    - In the Networking, create a security group and put necessary security configurations
    - Also, in the Load balance, allow App LB and create a new ALB.

## Step 6: Test your app
   - Wait for the service to succesffuly deploy. 
    - Then, go to the load balancer page in your console and copy the DNS URL.
    - Append /sports at the end of the URL and paste in your browser.

## Step 7: Create an API Gateway
 - Navigate to API Gateway on your console. 
    - Create a GET API.
    - After that, go to create resource.
    - Creat a resource /sports
    - Then, go to create a method
    - Create a GET request, with HTTP endpoint
    - Enable HTTP proxy integraion
    - In the Endpont URL
     `http:// the alb DNS endpoint /sports`
    - Then create method.


## Step 8: Deploy and Test the API Gateway
  - Deploy the API (From stage: "New Stage" to "Dev"), then deploy
    - To test the app, copy the invoke URL and append /sports at the end.

### You have successfully created a Dockerized app using the AWS ECS and AWS Fargate.

### REMEMBER TO DELETE ALL THE RESOURCES DEPLOYED.