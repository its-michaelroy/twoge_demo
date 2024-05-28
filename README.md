# Twoge

[Google Doc](https://docs.google.com/document/d/1H8H8iG8tLx-cUKVAzlM9yOGHkM3Gx_LcA9CHK7g8y6g)

Elon Musk, the renowned entrepreneur, is developing a new version of Twitter called "Twoge" where users are only allowed to tweet about Doge. While some may question the need for such an app. He has reached out to Codeplatoon to launch, maintain and deploy the app. 

Introducing **Twoge**, the latest buzz in the world of social media! Created by the genius of Elon Musk, Twoge is the only platform where you can share your thoughts about Doge, and nothing else.

Why Twoge, you ask? Well, for starters, the world needs more Doge content! We know you've been holding back your love for this beloved automobile brand, and Twoge is here to give you the perfect platform to express it all. From the classic Charger to the modern Challenger, Twoge is the place to share your favorite Doge models and stories with fellow enthusiasts.

But that's not all. With Twoge, you can connect with like-minded Doge fans from all over the world. Discuss your favorite models, share your experiences on the road, and even organize meetups and events with your fellow Twoge users.

And let's not forget the benefits of Twoge. With a platform dedicated solely to Doge, you'll never have to sift through irrelevant content again. No more baby photos, political rants, or food pics. Just pure, unadulterated Doge content all day, every day.


## Goal

As a DevOps engineer at Codeplatoon, you are supposed to deploy the app on AWS. He is particular about the following services to be used in deployment.

[Source code](https://github.com/chandradeoarya/twoge)
Full marks: 100

## Rubric

AWS Services and their purpose for Twoge
AWS EC2 (Elastic Compute Cloud) to host your twoge application.
AWS S3 (Simple Storage Service) to store your static files, such as images, videos, and other assets.
AWS IAM (Identity and Access Management) to manage service’s access and permissions to AWS resources.
AWS VPC (Virtual Private Cloud) to create a secure and isolated network environment for your application in public subnet.
AWS ALB (Application Load Balancer) to distribute incoming traffic across multiple EC2 instances.
AWS ASG (Auto Scaling Group) to automatically scale your EC2 instances up or down based on the demand coming from aspiring astronauts.
AWS SNS (Simple Notification Service) to receive notifications about your application's performance and health.
AWS RDS for database (if you want to be more hacky, you can do deploy postgres database on private subnet)

## Milestones

Create an Amazon VPC with two public subnets.
[Stretch: bonus point] If you want to be more hacky, you can create two private subnets. Configure a NAT gateway to allow instances in the private subnets to access the internet. And use servers in this subnet as a database. Otherwise simply use RDS postgres.
Host the static files like images and videos on S3 bucket.
Create an IAM role that allows public access  of S3 bucket.
Launch an EC2 instance with an Amazon Linux 2 AMI, using the IAM role you created in step 2. Install and configure your twoge application on the EC2 instance.  If desired use Chandra’s Twoge setup script
Create an Amazon ALB and configure it to route traffic to your EC2 instance. Add a listener rule to forward traffic to HTTP.
Create an Amazon ASG that automatically launches EC2 instances when traffic to your application exceeds a certain threshold. Configure the ASG to use the Amazon ALB as the load balancer.
Use Amazon SNS to receive notifications when the number of EC2 instances in your ASG increases or decreases. Configure an SNS topic to send email notifications to your email address.
Stop a server and SNS should send email notifications about server shut down.
Run the instance stress python script and show the ASG in play.

## Grading & Presentation
Grading will be done by presentation of work. You must also submit a document describing work (see below). The grading rubric is below. 

Present and explain an architecture diagram of your system
Demo, show, and explain each of the assessment milestones running on your AWS account.
Submit a document containing the following steps and code or commands.
All the commands used in the terminal for successful deployment of the application.
JSON file of S3 bucket policy.
Nginx configuration
Twoge daemon

## Grading Rubric 
Grading is pass/fail. 70% or higher (14 / 20) is a passing grade. You are able to retest if you do not pass.  

### Milestones
Marking Total (20 pts)

Milestone 1: 2 pts
Milestone 2: 2 pts
Milestone 3: 2 pts
Milestone 4: 2 pts
Milestone 5: 2 pts
Milestone 6: 2 pts
Milestone 7: 2 pts
Milestone 8: 2 pts
Milestone 9: 2 pts
Architecture Diagram & documentation of steps / commands: 2 pts

## Assessment Presentation Process:
Briefly summarize the task/project
Show diagrams & explain your architecture
Show twoge running & demo all milestone-related twoge features
Show relevant AWS console page stuff for each milestone
Run stress test, show results of stress test, or describe effort to run stress test.
Depending on time show any other relevant docs or bits of code, starting w/the most important/milestone-specific.

## Topics covered:
Linux commands 
Bash scripting 
EC2
S3
IAM
VPC
ALB
ASG
SNS

