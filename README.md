# Deploy-nodejs-docker-ec2-DynamoDB
Deployment of a Dockerized Node.js application on AWS EC2 with DynamoDB integration, accessible via public IP.

## About the Project:

This project demonstrates how to deploy a **pre-built Dockerized Node.js application** on an AWS EC2 instance and connect it to an AWS DynamoDB.

Instead of building the application manually, the Docker image is **pulled directly from Docker Hub** and executed using environment variables to establish a connection with the DynamoDB.

## Technologies Used:

- **Docker** → Container runtime
- **AWS EC2** → Hosting server
- **AWS DynamoDB** → NoSQL database
- **Docker Hub** → Pre-built image repository
- **Linux (Ubuntu)** → Server environment

## Prerequisites:

- AWS account
- EC2 instance (Ubuntu)
- DynamoDB

## Step 1: Launch EC2 Instance

1. Go to AWS Console → EC2
2. Launch Amazon Linux instance
3. Configure security group:
    - Allow SSH (22)
    - Allow HTTP (80)

![Project Screenshot](/images/instance1.png)
![Project Screenshot](/images/instance2.png)
![Project Screenshot](/images/instanceDone.png)

## Step 2: Setup RDS Database

1. Go to AWS Console → DynamoDB
2. Create Table 
3. Go to Explore item 
4. add manual data for testing then check data

![Project Screenshot](/images/DynamoDB1.png)
![Project Screenshot](/images/DynamoDB2.png)

## Step 3: Install Docker on EC2

1. connect EC2 instance via Git bash
2. Install Docker

```bash
sudo yum update
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
```

## Step 4: Pull Docker Image

1. Pull the per created image on the your server

```bash
sudo docker pull philippaul/ode-dynamo-app:02
#i will exaplin in next repo how to create docker image or how to push docker hub
```

## Step 5: Run Application Container

```bash
sudo docker run -d -p 80:3000 \
--name node-dynamo-app \
-e AWS_REGION=us-east-1 \
-e AWS_ACCESS_KEY_ID=<<YOUR-ACCESS-KEY>> \
-e AWS_SECRET_ACCESS_KEY=A0OVy6O4AwsLtND9a9k48dbWi2A+Ql/Ec4Lr3FDw \
philippaul/node-dynamodb-demo
```

## Step 7: Give Permission to the IAM user

1. Go to AWS IAM Console 
2. Click on the user then attach permission 
3. select the 3 permission → IAM, DynamoDB, EC2 full Access
4. then attach with Fully access 

![Project Screenshot](/images/IAM1.png)
![Project Screenshot](/images/IAM2.png)
![Project Screenshot](/images/IAM3.png)
![Project Screenshot](/images/IAM4.png)

## Step 8: Access Application

Open browser:

```
http://<EC2-PUBLIC-IP>
http://54.86.206.216/
```
![Project Screenshot](/images/AccessPage.png)

## Step 7: Verify Database Connection & Check Data

```bash
sudo docker run -it --rm mysql:8.0 \
mysql -h database-1.c2bguysa4229.us-east-1.rds.amazonaws.com -u admin -p
```

![Project Screenshot](/images/Data.png)
![Project Screenshot](/images/AccessData.png)

## Step 8: Delete All Resources

1. Go to AWS DynamoDB console
2. Then select the Table and click the delete
3. Go to the AWS EC2 console 
4. Then select instance which you want to delete
