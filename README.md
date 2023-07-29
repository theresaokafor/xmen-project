# xmen-project
This README file provides a detailed guide on how to deploy the provided HTML website on an EC2 instance while implementing various services to ensure successful deployment. The goal is to have a scalable and secure website accessible using your own domain name with HTTPS enabled.

## Project Overview

In this project, we will:

1. Deploy the provided HTML website on an EC2 instance.
2. Implement necessary AWS services, including VPC, security groups, ACM (Amazon Certificate Manager), record set, Route 53, Application Load Balancer (ALB), target group, and set up a server to ensure successful deployment.
3. Utilize a launch template to create auto scaling groups for efficient resource management.
4. Ensure that the website is accessible using your own domain name.
5. Enable secure communication to the site by enabling HTTPS.
6. Store the web files in a GitHub repository, and the deployment script should be able to download the files from GitHub repo.

## Steps for Deployment
### Prerequisites

Before proceeding, ensure you have the following:

- AWS Account with appropriate IAM permissions to create necessary resources.
- AWS CLI installed and configured with access to your AWS account.
- Domain name registered through Route 53 or another registrar.

### Step 1: Download the Web Files

Upload the web files provided into your GitHub repository.

### Step 2: Create Necessary AWS Resources

1. Create a 2-tier Virtual Private Cloud (VPC) with public and private subnets to host the EC2 instance and load balancer.

2. Set up security groups to control inbound traffic for the Application web-servers (open ports 80 & 443 to ALB security group)  and ALB (open ports 80 & 443 from anywhere).

3. Request an SSL/TLS certificate for your domain using AWS ACM.

4. Create a record set in Route 53 to associate your domain with the ALB's DNS name.

5. Create an Application Load Balancer (ALB) with a target group to forward traffic to the EC2 instance.

6. Set up a launch template to define the configuration for EC2 instances (App-servers) in the auto scaling group.

### Step 3: Deploy the Website on EC2 Instance (using user data on the Launch Template)

Use the provided deployment script to deploy the website on the EC2 instance:

```bash
#!/bin/bash
sudo su
yum update -y
yum install -y httpd
cd /var/www/html
wget https://github.com/theresaokafor/xmen-project/raw/main/xmen-main.zip
unzip xmen-main.zip
cp -r xmen-main/* /var/www/html/
rm -rf xmen-main xmen-main.zip
systemctl enable httpd
systemctl start httpd
```
### Step 4: Enable Auto Scaling

Configure an auto scaling group using the launch template created earlier to manage resources efficiently based on traffic and demand.

### Step 5: Set Up Your Domain Name

1. In Route 53, create a hosted zone for your domain.

2. Update the domain's DNS settings with the provided record set, linking it to the ALB's DNS name.

### Step 6: Enable HTTPS

Configure the ALB to use the SSL/TLS certificate from ACM, enabling HTTPS for secure communication.

## Conclusion

Following the above steps, you will have successfully deployed the provided HTML website on an EC2 instance with scalability, a custom domain name, and HTTPS enabled. Your website will be secure and efficiently managed using auto scaling groups. If you encounter any issues or need further assistance, refer to the AWS documentation or feel free to ask for help.

Enjoy your scalable and secure website deployment!
