+++
title = "7. AWS Config: Compliance as Code"
weight = 19
+++

### Create a Config Rule to alert on whether the SSM Agent is working

In this step we will add a config rule to our environment using an *AWS Managed* rule that will evaluate if EC2 instances in the account have a working AWS Systems Manager Agent.

1. Open the AWS Config console.
2. Click on **Rules** in the navigation menu at the left of the console.
3. Click on the **Add rule** button:
![](/images/lab_10_config_rule_add.png)
4. On the **Add Rule** screen type *ec2-instance-managed-by-systems-manager* in the **Find by rule name** search field.  Click on the displayed rule box:
![](/images/lab_10_find_config_rule.png)
5. In the **Trigger** section take note of the **Trigger type** (*When configuration changes*):
![](/images/lab_10_trigger_type.png)
6. Leave all other settings on their defaults and click on the **Save** button.
 ![](/images/lab_10b_ssmagentconfigrule.png)

{{% notice note %}}
You can create Config Rules to monitor a number of items within your infrastructure. Besides utilizing AWS managed Config rules you can also create custom rules using Lambda Functions. Located here in [Github](https://github.com/awslabs/aws-config-rules) are some sample Config rules you can create and implement using AWS Lambda.
{{% /notice %}}

### Test Config Rule by Deploying an EC2 Instance
---
To test the AWS Config rule we will now deploy a new EC2 instance.  
1. Open the **EC2** console.
2. In the navigation menu on the left, click on *Instances*, under the **INSTANCES** heading:
![](/images/lab_10_EC2_instances.png)
3. Click on the **Launch Instance** button:
![](/images/lab_10_launch_instance.png)
4. Choose the first Operating System choice in the list - **Amazon Linux 2 AMI (HVM)** - by clicking on the **Select** button next to it:
![](/images/lab_10_select_os.png)
5. On the **Step 2: Choose an Instance Type** screen tick the box next to **t3.large**, then click on the button **Review and Launch**:
![](/images/lab_10_instance_type.png)
6. On the **Review Instance Launch** screen click on the **Launch** button.
7. Select the **SSMLabKeys** key pair and tick the box to acknowledge the warning. Click on the **Launch Instances** button:
![](/images/lab_10_ec2_key_warning.png)

The EC2 instance will now be created for you.

8. Click on the **view Instances** button at the bottom right of the console. Look for the instance that has no name in the **Name** column (this will be the one you just created). Make a copy of the **Instance ID**:
![](/images/lab_10_instance-ID.png)
9. Open the AWS Config console.
   * Click on **Rules**.  The rule you deployed earlier will be shown.  click on the rule name (*ec2-instance-managed-by-systems-manager*):
![](/images/lab_10_deployed_rule.png)
   * If the EC2 instance has been deployed and running for several minutes the rule will have updated automatically to add the new EC2 resource that is non-compliant to the list.  Look for the Instance ID you noted down in step 8 (If it has not yet updated, click on the **Re-evaluate** button in the top left of the console, then refresh the web page until the rule flags the EC2 instance as **Noncompliant**):
![](/images/lab_10_noncompliant_instance.png)  

### Remediate Non-compliant Resources
---
Now we will remediate the non-compliant resource:

1. Open the CloudFormation console.
2. Click on the blue stack name **ConfigLabStack**:
![](/images/lab_10_cfn_stack_name.png)
3. Click on the **Outputs** tab.
4. Look for the **Key** called **EC2SSMRoleName** and copy the **Value** next to it to a text document (it will look similar to this - **ConfigLabStack-SSMConfigEC2LabRole-**, with some numbers and characters at the end):
![](/images/lab_10_ec2ssmrolename.png)
5. Open the AWS Config console.
6. Click on **Rules** in the navigation menu on the left of the console.
7. Click on the blue rule name **ec2-instance-managed-by-systems-manager**.
8. Click on the **Edit** button to edit the **ec2-instance-managed-by-systems-manager** Config rule:
![](/images/lab_10_edit_config_rule.png)
9. In the **Choose Remediation Action** section do the following:
  	* **Remediation action**: choose **AWS-AttachIAMToInstance**
  	* Click on the dropdown selection for **Resource ID parameter** and choose **InstanceId** (this will pass the non-compliant Instance ID to the remediation action)
	* In the **Parameters** section paste the value you copied in step 4 into the field next to **RoleName**
![](/images/lab_10_config_remediation.png)

10. Click on the **Save** button.

11. Click on the AWS Config rule name **ec2-instance-managed-by-systems-manager** and look at the non-compliant resource. Click on the selection box next to the EC2 instance you deployed earlier and then click on **Remediate**:
![](/images/lab_10_rule_remediation.png)
12. Open the AWS Systems Manager console and click on **Automation** in the left hand menu. You should see an Automation execution task listed that will attach an IAM role to the EC2 instance:
![](/images/lab_10_automation_ssm.png)
13. Restart the EC2 instance to quicken the process:
	* Go to the EC2 Console
	* Select the box next to your t3.large instance
	* Click on the **Actions** button and choose **Instance State**, then choose **Reboot**.
	* Click on the button **Yes, Reboot**.
14. Go back to the AWS Systems Manager console, and click on **Managed Instances** in the left hand menu. The EC2 instance should now be shown as a managed instance in the list (the deployed instance will have a blank **Name** field in the list).  **This step may take up to two minutes**.
15. Go back to the AWS Config console and click on **Rules**.
16. Click on the rule name **ec2-instance-managed-by-systems-manager**.
17. The EC2 instance will no longer be displayed in the **non-compliant** list.  Change the **Compliance Status** selection to **Compliant** and observe that the EC2 instance is now listed there.

#### *KEY CONCEPT:* What did we learn?
* How to create an AWS Config Rule to evaluate if instances are managed by SSM
* Importance of having the right IAM Role assigned for the instance to report to SSM.
* How to use AWS Systems Manager Automation documents to remediate non-compliant Instances.

**Congratulations!  You've now completed Lab 2!**

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.7.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}


 
