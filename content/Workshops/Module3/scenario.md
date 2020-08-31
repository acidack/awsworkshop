+++
title = "1. Scenario"
date = 2019-11-18T17:11:28+11:00
weight =2
+++

## Overview

In this workshop, you will have an environment provisioned consisting of two Amazon Linux web servers behind an application load balancer. The web servers will be running a PHP web site that contains several vulnerabilities. You will then use AWS Web Application Firewall (WAF), Amazon Inspector and AWS Systems Manager to identify the vulnerabilities and remediate them.

### Scenario

Welcome to Widgets LLC! You have just joined the team and your first task is to enhance security for the company website. The site runs on Linux, PHP and Apache and uses an EC2 instance and autoscaling group behind an Application Load Balancer (ALB). After an initial architecture assessment you have found multiple vulnerabilities and configuration issues. The dev team is swamped and will not be able to remediate code level issues for several weeks. Your mission in this workshop round is to build an effective set of controls that mitigate common attack vectors against web applications, and provide you with the monitoring capabilities needed to react to emerging threats when they occur.

- Level: Intermediate - Advanced
- Duration: 2 hours

### Workshop Architecture

![Architecture](/images/mod4architecture.png)

You can now proceed to the [Assess Phase](../perimeter-assess).