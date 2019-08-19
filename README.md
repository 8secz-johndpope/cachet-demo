#	Status Page using Cachet

Cachet is an open source Status Page which has a great status page and a powerful API

This document will walk you through the process of deploying Cachet on AWS ECS. The Cloudformation templates you find in this repository will help you start with a 
VPC 
Subnets in 3 AZ’s in the Region
IAM Roles
Route Tables 
RDS Secrets
RDS MySQL Instance 
ECS Task Definition
ALB’s
ECS Fargate Service

#	Purpose
A Status page is very useful for your business and is application to any internet services
Cachet Team have done a great job in developing such a wonderful tool and decided to share with us.
This project is inspired by https://docs.cachethq.io, https://github.com/CachetHQ/Docker
I wanted to setup a Status page as well as a monitoring solution on Lambda(Coming soon) and post the metrics onto Cachet’s REST API. (https://docs.cachethq.io/v1.0/reference#ping)

#	Deployment
I have created few Cloudformation templates which will help you setup the Cachet in your on AWS VPC of your choice. Or you may create your own VPC network from scratch.
The project is dockerized and we will deploy it on ECS Fargate.

The ecs-cfn.yml will create, TaskDefinition, ALB, ECSCluster and maintain your desired state
Modify the template according to the best needs of your environment for Autoscaling.

With the Cloudformation Autoscaling parameters defined in the template, it’s super easy to modify the cluster anytime.

You will have to provide DNSHostedZone and few other parameters when your create the stack.

#	Commands
	> aws cloudformation create-stack --stack-name cachet-demo --template-body file://cachet-master.yml --capabilities CAPABILITY_NAMED_IAM

	> aws cloudformation update-stack --stack-name cachet-demo --template-body file://cachet-master.yml --capabilities CAPABILITY_NAMED_IAM

Once the Cloudformation stacks are created, you will have to do the initial configuration for Cachet. 
Access you Cachet implementation with the URL you may find in cloudformation stack outputs


To test the Cachet Docker project locally
	docker run --name cachet -e DB_HOST=<DatabaseHost> -e DB_USERNAME=<username> -e DB_PASSWORD=<password> -e APP_KEY=<base64AppKey> -p 8000:8000 cachethq/docker:latest


#	Secrets
Here in these templates, I have used AWS Secrets manager to create secrets for RDS instance as well secrets for the Cachet app which will be passed as ENV variable in ECS Task definition.

#	Autoscaling
The current autoscaling policy will scale the cluster according to the CPUUtilization of the containers. You may modify the template to add different conditions such as ALB Request count or based on Cloudwatch alarm.

