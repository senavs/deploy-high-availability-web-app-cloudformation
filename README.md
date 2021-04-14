# Deploy a High Availability Web App With Cloudformation
Udacity Cloud DevOps Engineer Nanodegree Program - Project 2

## Introduction
As your final project, you'll be faced with a real scenario.

Creating this project will give you the hands-on experience you need to confidently talk about infrastructure as code. So, for that reason, we have chosen a realistic scenario where you deploy an application (Apache Web Server) and you also pick up code (JavaScript and HTML) from S3 Storage and deploy it in the appropriate folder on the web server.

There will be two parts to this project:
  - Diagram: You'll first develop a diagram that you can present as part of your portfolio and as a visual aid to understand the CloudFormation script.
 - Script (Template and Parameters): The second part is to interpret the instructions and create a matching CloudFormation script.


## Problem
Your company is creating an Instagram clone called Udagram. Developers pushed the latest version of their code in a zip file located in a public S3 Bucket.

You have been tasked with deploying the application, along with the necessary supporting software into its matching infrastructure.

This needs to be done in an automated fashion so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.

## Project Requirements

### Server specs
  1. You'll need to create a **Launch Configuration** for your application servers in order to deploy four servers, two located in each of your private subnets. The launch configuration will be used by an auto-scaling group.
  2. You'll need two vCPUs and at least 4GB of RAM. The Operating System to be used is Ubuntu 18. So, choose an Instance size and Machine Image (AMI) that best fits this spec.
  3. Be sure to allocate at least 10GB of disk space so that you don't run into issues.


### Security Groups and Roles
  1. Since you will be downloading the application archive from an **S3 Bucket**, you'll need to create an **IAM Role** that allows your instances to use the S3 Service.
  2. Udagram communicates on the default `HTTP Port: 80`, so your servers will need this inbound port open since you will use it with the **Load Balancer** and the **Load Balancer Health Check**. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.
  3. The load balancer should allow all public traffic `(0.0.0.0/0)` on `port 80` inbound, which is the default `HTTP port`. Outbound, it will only be using port 80 to reach the internal servers.
  4. The application needs to be deployed into private subnets with a **Load Balancer** located in a public subnet.
  5. One of the output exports of the **CloudFormation** script should be the public URL of the **LoadBalancer**. Bonus points if you add `http://` in front of the load balancer **DNS Name** in the output, for convenience.


### Starter Code
You will find starter code for the project in the [.yaml](https://video.udacity-data.com/topher/2019/July/5d391e8b_final-project-starter/final-project-starter.yml) file, also located in the Resources section of the left sidebar of this page.

## Other Considerations
1. You can deploy your servers with an SSH Key into Public subnets while you are creating the script. This helps with troubleshooting. Once done, move them to your private subnets and remove the SSH Key from your Launch Configuration.
2. It also helps to test directly, without the load balancer. Once you are confident that your server is behaving correctly, increase the instance count and add the load balancer to your script.
3. While your instances are in public subnets, you'll also need the SSH port open (port 22) for your access, in case you need to troubleshoot your instances.
4. Log information for UserData scripts is located in this file: cloud-init-output.log under the folder: /var/log.
You should be able to destroy the entire infrastructure and build it back up without any manual steps required, other than running the CloudFormation script.
5. The provided UserData script should help you install all the required dependencies. Bear in mind that this process takes several minutes to complete. Also, the application takes a few seconds to load. This information is crucial for the settings of your load balancer health check.
6. It's up to you to decide which values should be parameters and which you will hard-code in your script.
7. See the provided supporting code for help and more clues.
8. If you want to go the extra mile, set up a bastion host (jump box) to allow you to SSH into your private subnet servers. This bastion host would be on a Public Subnet with port 22 open only to your home IP address, and it would need to have the private key that you use to access the other servers.

  > Last thing: Remember to delete your CloudFormation stack when you're done to avoid recurring charges!

## Solution

### URL (It will be disabled soon)
http://udaci-loadb-1lhrn0gmxlwf-1762248331.us-east-1.elb.amazonaws.com/

### Architecture
![Architecture](https://raw.githubusercontent.com/senavs/deploy-high-availability-web-app-cloudformation/master/architecture/diagram.jpg)

### Templates
- [Netwoork](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/network.yml)
- [Role](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/role.yaml)
- [Service](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/service.yml)

### Parameters
- [Netwoork](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/network-parameters.json)
- [Role](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/role-parameters.json)
- [Service](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/IaC/service-parameters.json)

### Set up
Get into project folder `deploy-high-availability-web-app-cloudformation`
```sh
cs deploy-high-availability-web-app-cloudformation
```

Create stacks
```sh
bash scripts/create.sh udacity-project-02-network ./IaC/network.yml ./IaC/network-parameters.json

bash scripts/create.sh udacity-project-02-role ./IaC/role.yaml  ./IaC/role-parameters.json

bash scripts/create.sh udacity-project-02-service ./IaC/service.yml ./IaC/service-parameters.json
```

Update stacks
```sh
bash scripts/update.sh udacity-project-02-network ./IaC/network.yml ./IaC/network-parameters.json

bash scripts/update.sh udacity-project-02-role ./IaC/role.yaml  ./IaC/role-parameters.json

bash scripts/update.sh udacity-project-02-service ./IaC/service.yml ./IaC/service-parameters.json
```

Delete stacks
```sh
bash scripts/delete.sh udacity-project-02-network

bash scripts/delete.sh udacity-project-02-role

bash scripts/delete.sh udacity-project-02-service
```

### Web Application
![web](https://raw.githubusercontent.com/senavs/deploy-high-availability-web-app-cloudformation/master/images/05%20-%20outputs/01%20-%20Web%20Application.png)

### Jump Box
![jump box](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/raw/master/images/05%20-%20outputs/02%20-%20Jump%20Box.png)

### Jump Box using ssh to access EC2 in private subnet
![jump box](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/raw/master/images/05%20-%20outputs/03%20-%20Jump%20Box%20shh%20to%20Private%20Web%20Application.png)

### AWS prints and other images
[Other images](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/tree/master/images)

### Load Balancer output with URL
```yml
Outputs:

  LoadBalancer:
    Description: A reference to the created LoadBalancer
    Value: !Join [ "", [ 'http://', !GetAtt LoadBalancer.DNSName ]]
    Export:
      Name: LoadBalancerURL
```

![output with url](https://github.com/senavs/deploy-high-availability-web-app-cloudformation/blob/master/images/03%20-%20service/05%20-%20outputs.png)