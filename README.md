
# Week-9 Assignment: AWS Web Application Deployment

This project is part of the Clarusway Infrastructure Bootcamp. The goal is to deploy a highly available website using AWS services.

## Technologies Used

- Amazon S3 for static assets
- EC2 Auto Scaling Group with Launch Template for NGINX servers
- Application Load Balancer for traffic distribution
- Bash scripting (User Data)
- Amazon Linux 2

## Part 1: S3 Setup - Static Website Hosting

- Bucket name: abdullah-clarusway-assets
- Region: eu-north-1
- Files uploaded: index.html, logo.png, sda.png

Bucket policy used:
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::abdullah-clarusway-assets/*"
  }]
}

S3 website URL:
http://abdullah-clarusway-assets.s3-website.eu-north-1.amazonaws.com

curl check:
curl -I http://abdullah-clarusway-assets.s3-website.eu-north-1.amazonaws.com

## Part 2: Auto Scaling Group

Launch Template User Data:
#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
systemctl enable nginx
aws s3 cp s3://abdullah-clarusway-assets/index.html /usr/share/nginx/html/index.html

ASG Configuration:
- Min: 1
- Max: 3
- Desired: 2
- Health Checks: EC2 and ELB

## Part 3: Application Load Balancer

ALB Settings:
- Internet-facing
- HTTP listener on port 80
- Target Group with health check path: /

Testing:
for i in {1..5}; do curl -s http://<ALB-DNS> | grep hostname; done

## Success Criteria Met

- Website loads via S3 endpoint
- Website loads via ALB endpoint
- ASG replaces instances on termination
- All assets load correctly

## Cleanup

- Deleted S3 bucket
- Terminated ASG
- Removed ALB

## Screenshots

- s3-url-screenshot.png
- s3-curl-200ok.png
- asg-running-instances.png
- alb-dns-screenshot.png
- alb-curl-response.png
