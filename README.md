# Twoge Deployment on AWS

## Table of Contents

1. [Assessment Presentation Process](#assessment-presentation-process)
2. [Project Summary](#project-summary-part-1)
3. [Architecture Overview](#architecture-overview-part-2)
4. [Architecture Diagram Description](#architecture-diagram-description-part-3--4)
5. [Steps to Stop an EC2 Instance Using the AWS Management Console](#steps-to-stop-an-ec2-instance-using-the-aws-management-console)
6. [Stress Test](#stress-test-part-5)
7. [Terminal Commands for Deployment](#terminal-commands-for-deployment)
8. [Configuration Files](#configuration-files)

## Assessment Presentation Process

1. Briefly summarize the task/project
2. Show diagrams & explain your architecture
3. Show twoge running & demo all milestone-related twoge features
4. Show relevant AWS console page stuff for each milestone
5. Run stress test, show results of stress test, or describe effort to run stress test.
6. Depending on time show any other relevant docs or bits of code, starting w/the most important/milestone-specific.

## Project Summary (Part 1)

The task involves deploying and managing the Twoge application on AWS infrastructure. The project utilizes various AWS services to ensure scalability, reliability, and efficiency. Key tasks include setting up a Virtual Private Cloud (VPC) with subnets, hosting static files on S3, configuring IAM roles for access management, deploying the application on EC2 instances, managing traffic using an Application Load Balancer (ALB), implementing an Auto Scaling Group (ASG) for dynamic scaling, and setting up Simple Notification Service (SNS) for monitoring and notifications.

## Architecture Overview (Part 2)

![Architecture Diagram](architecture-diagram.png)

### AWS Services Used

- **AWS EC2 (Elastic Compute Cloud):** Hosts the Twoge application.
- **AWS S3 (Simple Storage Service):** Stores static files, such as images and videos.
- **AWS IAM (Identity and Access Management):** Manages access and permissions to AWS resources.
- **AWS VPC (Virtual Private Cloud):** Creates a secure and isolated network environment for the application.
- **AWS ALB (Application Load Balancer):** Distributes incoming traffic across multiple EC2 instances.
- **AWS ASG (Auto Scaling Group):** Automatically scales EC2 instances based on demand.
- **AWS SNS (Simple Notification Service):** Sends notifications about application performance and health.
- **AWS RDS (Relational Database Service):** Manages the PostgreSQL database (optional).

## Architecture Diagram Description (Part 3 & 4)

1. **VPC (Virtual Private Cloud):**
   - Create a VPC with two public subnets. Optionally, add private subnets with a NAT gateway for internal services.
   - This provides a secure and isolated network environment for the application.

2. **S3 (Simple Storage Service):**
   - Host static files such as images, videos, and other assets.
   - An IAM role is configured to allow public access to the S3 bucket.

3. **EC2 (Elastic Compute Cloud):**
   - Launch EC2 instances to host the Twoge application.
   - Install and configure the application using a setup script.

4. **IAM (Identity and Access Management):**
   - Manage permissions and access to AWS resources.
   - Create roles to allow EC2 instances to access S3 and other necessary resources.

5. **ALB (Application Load Balancer):**
   - Distribute incoming traffic across multiple EC2 instances to ensure availability and reliability.
   - Add a listener rule to forward HTTP traffic to the appropriate EC2 instances.

6. **ASG (Auto Scaling Group):**
   - Automatically scale the number of EC2 instances up or down based on CPU utilization and traffic demand.
   - Ensures the application can handle varying levels of traffic efficiently.

7. **SNS (Simple Notification Service):**
   - Receive notifications about the application's performance and health.
   - Send email notifications for scaling events and server shutdowns.

## Steps to Stop an EC2 Instance Using the AWS Management Console

1. Go to the AWS Management Console and log in with your credentials.
    - Navigate to the EC2 Dashboard:

2. In the AWS Management Console, click on "Services" in the top-left corner.
    - Under "Compute," select "EC2."
    - Select Your Instance:

3. In the EC2 Dashboard, click on "Instances" in the left-hand menu.
    - Locate the instance you want to stop from the list of running instances.
    - Stop the Instance:

4. Select the instance by clicking the checkbox next to its instance ID.
    - Click on the "Instance state" button at the top.
    - From the dropdown menu, select "Stop instance."
    - Confirm the Action:

5. A confirmation dialog will appear. Confirm that you want to stop the instance.

## Stress Test (Part 5)

**Run the instance stress python script and show the ASG in play.**

```python
#!/usr/bin/env python
"""
Produces load on all available CPU cores
"""

from multiprocessing import Pool
from multiprocessing import cpu_count

def f(x):
    while True:
        x*x

if __name__ == '__main__':
    processes = cpu_count()
    print ('utilizing %d cores\n' % processes)
    pool = Pool(processes)
    pool.map(f, range(processes))
```

- JSON file of S3 bucket policy.

```json
{
    "Version": "2012-10-17",
    "Id": "Policy14174463327",
    "Statement": [
        {
            "Sid": "Stmt14174463327",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": [
                "arn:aws:s3:::michael-s3-bucket",
                "arn:aws:s3:::michael-s3-bucket/*"
            ]
        }
    ]
}
```

- Nginx configuration

```nginx
#located at /etc/nginx/conf.d/twoge.conf
server {
    listen 80;
    server_name 64.11.164.237;

    location / {
        proxy_pass http://0.0.0.0:9998;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

- Twoge daemon or user-data script

```bash
#!/bin/bash
#located at /etc/systemd/system/twoge.service

sudo yum update -y
sudo yum install git -y
git clone  https://github.com/chandradeoarya/twoge
cd twoge
sudo yum install python-pip -y
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

echo DATABASE_URL = 'postgresql://postgres:password@michael-db.us-east-1.rds.amazonaws.com:5432/mikes_db' > .env

echo '
Description=Gunicorn instance to serve twoge

Wants=network.target
After=syslog.target network-online.target

[Service]
Type=simple
WorkingDirectory=/home/ec2-user/twoge
Environment="PATH=/home/ec2-user/twoge/.venv/bin"
ExecStart=/home/ec2-user/twoge/.venv/bin/gunicorn app:app -c /home/ec2-user/twoge/gunicorn_config.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target' > twoge.service

sudo cp twoge.service /etc/systemd/system/twoge.service

sudo systemctl daemon-reload

sudo systemctl enable twoge

sudo systemctl start twoge

sudo systemctl status twoge
```

## Configuration Files

## Connect to EC2 Instance

```bash
ssh -i "your-key-pair.pem" ec2-user@your-ec2-public-dns
```

## Run the Setup Script

- Do the following once you have the twoge.sh script

```bash
chmod +x twoge.sh
./twoge.sh
```

## Configure and Start Nginx

```nginx
sudo cp twoge.conf /etc/nginx/conf.d/
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

## Create and Start the Twoge Service

```bash
sudo cp twoge.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable twoge
sudo systemctl start twoge
sudo systemctl status twoge
```
