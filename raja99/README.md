# Architecture-----1
I have designed and implemented the following architecture, and successfully completed a proof of concept.

![ECS-Fargate1](https://github.com/INDALARAJESH/ecs-task-defination/assets/109213968/a0e29eaf-1e5c-4404-a619-089244944603)




# Creating a Docker File

#step:1
take ubuntu instance (t2.micro)
update the packages

sudo apt update

#step:2 
install docker

sudo apt install docker.io
sudo chmod 666 var/run/docker.sock

#step:3

 download html static pages

curl -O https://www.free-css.com/assets/files/free-css-templates/download/page296/inance.zip

#step:4
unzip the file

sudo apt install unzip

unzip inance-html

#step:5 

create the docker file:

vi Dockerfile

#Base Image
FROM nginx:latest

#Remove the files
RUN rm ./usr/share/nginx/html/*

#Working Dir
WORKDIR /app

#copy the files in html folder
COPY ./inance-html/ /usr/share/nginx/html/

#expose container port 80
EXPOSE 80

provide the access keys & secret key for aws configuration

aws configure
enter access keys

step:6
build the docker image and push to aws ecr

aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/u9n2k9v9
docker build -t dxc .
docker tag dxc:latest public.ecr.aws/u9n2k9v9/dxc:latest
docker push public.ecr.aws/u9n2k9v9/dxc:latest



# Terraform Configuration Overview

This repository contains Terraform files for deploying and managing AWS resources. Below is an overview of each file and its purpose.

## File Descriptions

1. **`alb.tf`**
   - **Purpose**: Manages AWS Application Load Balancer (ALB) resources.
   - **Resources**:
     - `aws_alb`: The load balancer.
     - `aws_alb_listener`: Listeners for the load balancer.
     - `aws_alb_target_group`: Target groups for traffic distribution.

2. **`provider.tf`**
   - **Purpose**: Configures the Terraform provider for AWS.
   - No specific resources are defined.

3. **`vpc.tf`**
   - **Purpose**: Defines resources for AWS Virtual Private Cloud (VPC).
   - **Resources**:
     - `aws_subnet`, `aws_vpc`: Defines the VPC and its subnets.
     - `aws_route`, `aws_route_table`, `aws_route_table_association`: Routing configurations.
     - `aws_eip`, `aws_nat_gateway`, `aws_internet_gateway`: Internet and NAT gateways.

4. **`variable.tf`**
   - **Purpose**: Declares variables used in the Terraform configuration.
   - **Variables Defined**: 8

5. **`security_group.tf`**
   - **Purpose**: Manages AWS security group rules.
   - **Resources**:
     - `aws_security_group`: Security group rules.

6. **`output.tf`**
   - **Purpose**: Declares outputs for the Terraform configuration.
   - **Outputs Defined**: 1
   - **Outputs**: The output file provides the DNS name of the Load Balancer. When you enter this DNS name directly into a browser, it displays a static page. Additionally, accessing DNS_Name/about.html will show another static page.


7. **`iam_role.tf`**
   - **Purpose**: Manages AWS IAM roles and policy attachments.
   - **Resources**:
     - `aws_iam_role`, `aws_iam_role_policy_attachment`: IAM roles and policy attachments.

## Usage

Clone the repository from github and configure aws cli with using profile or env variabales and make sure that terraform and AWS cli is installed in your loacal machine or any server

1.Configure AWS CLI with a Profile:

Open a terminal and run the following command:

aws configure --profile omedo
Ensure that the profile you specify in the profile attribute corresponds to the profile configured in your AWS CLI (~/.aws/config)

2. Using Environment Variables:
Set the AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables in your terminal session or script before running Terraform
export AWS_ACCESS_KEY_ID="your-access-key-id"
export AWS_SECRET_ACCESS_KEY="your-secret-access-key"


To use these Terraform files:
1. Ensure you have Terraform installed and AWS access configured.
2. Run `terraform init` to initialize the Terraform environment.
3. Run `terraform validate` to check the validity of the Terraform code.
4. Use `terraform plan` to review the changes to be applied.
5. Apply the changes with `terraform apply`.


