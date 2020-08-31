+++
title = "Workshops"
date = 2019-11-18T08:29:21+11:00
weight = 3
chapter = false
disableToc = "false"
+++

## Workshops

These workshops are designed to help you get familiar with AWS Security and Operational services so you can improve your security and compliance objectives. You'll be working with services such as AWS Systems Manager (operational management), AWS Config (configuration change management), Amazon Inspector (vulnerability & behavior analysis) and AWS WAF (web application firewall). 

At the start of each workshop you will be able to watch a short video that will provide you an overview and a real world example.

| Topic | Description | Skill Level | Time |
|-----------|---------|---------|---------|
|[Eliminate Bastion Hosts with Systems Manager](/workshops/module1) | In this session, you will configure AWS Systems Manager Session Manager to provide secure interactive access to your managed instances without the need to expose inbound ports, manage bastion hosts, or manage SSH keys. You will learn how Session Manager works by default and will progressively increase the security posture of your environment by enabling enhanced session encryption, configuring session logging and reducing default permissions. | Beginner - Intermediate | 1 - 2 hours |
|[Security Through Good Governance](/workshops/module2)| In this session, you will leverage Systems Manager and AWS Config to enforce governance across your AWS resources. You will collect inventory from your instances, automate patch management, ensuring consistency across your instances and automating compliance enforcement.   | Beginner - Intermediate | 1 - 2  hours |
|[Protecting Workloads from the Instance to the Edge](/workshops/module3)| In this session, you will build an environment consisting of two Amazon Linux web servers behind an application load balancer. The web servers will be running a PHP web site that contains several vulnerabilities. You will then use AWS Web Application Firewall (WAF), Amazon Inspector and AWS Systems Manager to identify the vulnerabilities and remediate them.   | Advanced | 2 - 3 hours |
|[AWS Secrets Manager with Amazon RDS and AWS Fargate](/workshops/module4)| In this session, you will access the RDS database with Secrets Manager. You will then use Secrets Manager to rotate the data base password. You will then use Secrets Manager to access the database again to show that you can continue to access the data base after the rotation. In the second phase of the lab, you will extend your use of Secrets Manager into an AWS Fargate container. You will create an Amazon ECS task definition to pass secrets to the Fargate container and then launch the Fargate container. You will then SSH into the container to show that the secret was passed to the container and that you can access the RDS data base. | Intermediate - Advanced |  1 hour |
