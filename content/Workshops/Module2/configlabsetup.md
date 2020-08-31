+++
title = "6. AWS Config Lab Setup"
weight = 17
+++
### Create a Trail in CloudTrail
---
1. Open the **CloudTrail** service console.
2. Click on **Trails** in the navigation menu on the left.
3. Click on the **Create Trail** button, to create a trail for this lab.
    ![Create Trail](/images/lab_9a_CreateTrail.png)

4. Apply the following settings and create the trail
    * **Trail name**: *management-tools-workshop*
    * **Apply trail to all regions**: *Yes*
    * **Read/Write Events**: *All*
![](/images/lab_9a_trailname.png)
    * **Data Events**:
      * Check the box next to *Select all S3 Buckets in your account*
    * **Create a new S3 Bucket**:*Yes*
      * **S3 bucket**: *security-workshop-\(*today's date*\)-\(*yourmobilenumber*\)-*
      Example: *security-workshop-10062020-123456789*.
![](/images/lab_9a_s3bucketdetails.png)

      * **Note down your bucket name as you will need it when deploying lab resources later**.

{{% notice info %}}
We are using mobile phone number at the end to ensure that we create a unique bucket per user. For more information on Bucket Restrictions and Limitations click [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html)
{{% /notice %}}

5. Click on the **Create** button at the bottom right of the console.  You will now see the new trail listed.
![](/images/lab_9a_newtraillisted.png)
6. We will now configure the new trail to send logs to CloudWatch to make searching logs easier.
    * Click on the trail name.
    * Scroll down to the **CloudWatch Logs** section and click on **Configure**.
    ![Add CloudWatch Log to Trail](/images/lab_9a_ConfigCWLogCloudtrail.png)
    * In the **New or existing log group** section, type the following name *SecurityWorkshop/CloudTrail*, then click on the **Continue** button.
![](/images/lab_9a_cloudwatch_log_name.png)
    * On the next screen click on the **Allow** button. This grants CloudTrail the ability to assume a role to write to CloudWatch Logs.
![](/images/lab_9a_cloudwatch-role.png)

**Congratulations! You have now created a CloudTrail trail and added a CloudWatch log for the API activity in your AWS Account.**

### Turn on AWS Config
---
AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.

1. Open the **AWS Config** console.
2. Click on the **Get Started** button.
  * Leave all settings on their default choices and click on the **Next** button – This will create an S3 bucket, a role for the Config service, and begin recording configuration history for resources in the account:
  ![](/images/lab_9a_config_setup.png)
  * On the **AWS Config Rules** screen click on the **Next** button to skip selecting any AWS Config Rules for the moment.
  * On the **Review** screen click on the **Confirm** button.

**Congratulations!  AWS Config is now turned on in your account and is tracking configuration history.**

### Deploy Components for Lab
---

1. [Click here To deploy lab resources into your account](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=ap-southeast-2&stackName=ConfigLabStack&templateURL=https://patesumi-webcontent.s3-ap-southeast-2.amazonaws.com/ConfigLabStack.yml)

2. On the **Create stack** page leave all settings on their defaults and click on the **Next** button at the bottom right of the console.
3. In the **Parameters** section:
   * for **BucketName** enter the name of the S3 bucket that you created earlier during the CloudTrail configuration setup. Leave the TopicEmail field as it is.
4. Click on the **Next** button to continue.
5. On the **Configure stack options** screen click on the **Next** button to continue.
6. On the **Review** screen scroll to the bottom.  Tick the box next to **I acknowledge that AWS CloudFormation might create IAM resources**, then click on the **Create stack** button:
![](/images/lab_9a_config_iam_role.png)
7. Wait for the stack to deploy.  This will typically take up to five minutes to complete.

**The AWS Config Lab environment has now been created.  Please now move on to Part 7 of the lab.**

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.6.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}