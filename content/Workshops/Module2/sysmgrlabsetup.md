+++
title = "1. AWS Systems Manager Lab Setup"
weight = 11
+++
## Lab Setup

In this part of the lab you will set up the necessary resources in your environment to proceed with the Systems Manager labs.

### Create an EC2 Key Pair

1. Use your administrator account to access the Amazon EC2 console at <https://console.aws.amazon.com/ec2/>.

	{{% notice note %}}
  In 2019 the EC2 console was updated to have a consistent look and feel with other AWS Services, and to improve the user experience.
  This lab guide shows the new and improved console experience. You can enable and disable this new look based on your preferences.
  {{% /notice %}}

![](/images/lab_1c_EC2NewConsole.png)

2. In the left-hand menu look for the **Network & Security** section, and click on **Key Pairs**. Now click on the **Create Key Pair** button at the top right of the console.

![](/images/lab_1c_KeyPairLink.png)

3. In the **Create Key Pair** dialog box, type a **Key pair name** such as `SSMLabKeys`, leave the file format setting on the default of **pem** and then click on **Create key pair**.

![](/images/lab_1c_KeyPairCreate.png)

4. **Save the** `SSMLabKeys.pem` **file** to your local machine.
**Note:** You will not need the keys for this lab, they are only required for the CloudFormation Stack to deploy successfully (below, in the section: *To deploy the lab infrastructure*).


#### *KEY CONCEPT:* Management Tools - AWS Systems Manager ![](/images/_index_SSM_logo.png)  

[AWS Systems Manager](https://aws.amazon.com/systems-manager/features/) is a collection of features that enable IT Operations, some of which we will explore throughout this lab.  
There are [set up tasks](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) and [prerequisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-prereqs.html) that must be satisfied prior to using Systems Manager to manage your EC2 instances or on-premises systems in [hybrid environments](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html).  

  * Verify that your instances run a supported operating system.
  * For EC2 instances, create an IAM instance profile and attach it to your machines.
  * For on-premises servers and VMs, create an IAM service role for a hybrid environment.
  * Verify that you are allowing HTTPS (port 443) outbound traffic to the Systems Manager endpoints.
  * (Recommended) Create a VPC endpoint in Amazon Virtual Private Cloud to use with Systems Manager.
  * On on-premises servers, VMs, and EC2 instances created from AMIs that are not supplied by AWS, install a Transport Layer Security (TLS) certificate.
  * For on-premises servers and VMs, register the machines with Systems Manager through the managed instance activation process.
  * Install or verify installation of SSM Agent on each of your managed instances.


SSM Agent is installed by default on (base Amazon-managed AMIs):  
  * Windows Server 2008-2012 R2 AMIs published in November 2016 or later
  * Windows Server 2016 and 2019
  * Amazon Linux
  * Amazon Linux 2
  * Ubuntu Server 16.04
  * Ubuntu Server 18.04
  * Amazon ECS-Optimized

### Setting up Systems Manager
---
1. Use your account to access the [Systems Manager](https://console.aws.amazon.com/systems-manager/) console.
2. Choose **Managed Instances** from the navigation menu on the left. If you have satisfied the prerequisites for Systems Manager, you will arrive at the **AWS Systems Manager Managed Instances** page.
    * As a user with AdministratorAccess permissions, you already have [User Access to Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-access-user.html).
    * The Amazon Linux AMIs used to create the instances for this lab are dated 2017.09. They are [supported operating systems](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-supported-oses.html) and have the [SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) installed by default.


AWS Systems Manager now offers a _Quick Setup_ method to simplify configuring your instances to be managed.  
  * Click on the [Quick Setup](https://console.aws.amazon.com/systems-manager/quick-setup) link at the top of the navigation menu on the left.
  * Review the **Permissions (Required)** section. Note that there is a default role, and you can also specify your own role for more granular permissions.
  * Keep the selections **Instance Profile role** and **Assume role for Systems Manager** on the **Use the default role** selections.
  * Review the **Quick Setup options** section
    * Ensure that the **Update Systems Manager (SSM) Agent every two weeks** option is checked. Leave all other options unchecked.

      ![](/images/lab_3a_quick-setup-options.png)

  * In the **Targets** section, define targets for systems manager setup:
    * Under **Target selection method**, select **Choose all instances in the current AWS account and region**
  * Select the **Enable** button.  

  ![](/images/lab_3a_quick-setup-button.png)

### To deploy the lab infrastructure:  

1. Deploy the infrastructure with CloudFormation -
  [Click here To Deploy Lab into your Account](https://console.aws.amazon.com/cloudformation/home#/stacks/new?region=ap-southeast-2&stackName=SSMPatchLabStack1&templateURL=https://patesumi-webcontent.s3-ap-southeast-2.amazonaws.com/module2/SSMLabStack.yml)
![](/images/lab_2a_CFCreate-Stack.png)
2. On the **Create Stack** page, leave all settings on their defaults and click on **Next**.
3. In the **Specify stack details** section, accept the predefined stack name `SSMPatchLabStack1`.
4. In the **Parameters** section:
   * Specify the **InstanceProfile** as `AmazonSSMRoleForInstancesQuickSetup`. **MAKE SURE TO ENTER THIS EXACTLY AS WRITTEN OR THE STACK WILL NOT DEPLOY!**
   * Leave **InstanceTypeApp** and **InstanceTypeWeb** as the default free-tier-eligible t2.micro value.
   * Select the EC2 **KeyName** you defined earlier from the list (**SSMLabKeys**).
	 * Enter the loop-back IP address `127.0.0.1/32` in **SourceLocation**.
   * Define the **Workload Name** as `Prod`.
   * Click on **Next**.
  ![](/images/lab_2a_stack-details.png)
5. On the **Configure stack options** page under **Tags**, type **Owner** in the **Key** field, and enter your name in the **Value** field.
6. Leave all other sections unmodified. Scroll to the bottom of the page and click on **Next**.
  ![](/images/lab_2a_stack-options.png)
7. On the **Review** page, review your choices and then scroll down and click on **Create stack**.
  ![](/images/lab_2a_stack-review.png)
  ![](/images/lab_2a_create-stack.png)
8. On the CloudFormation console page your stack deployment will now begin.
9. Choose the **Events** tab for your selected workload to see the activity log from the creation of your CloudFormation stack.  Deployment will take about five minutes.
![](/images/lab_2a_events-tab.png)

### Check Deployed Instances
When the **Status** of your stack displays `CREATE_COMPLETE` in the filter list, you will have created a representation of a typical 'lift and shift' 2-tier application migrated to the cloud.
![](/images/lab_2b_create_complete.png)
1. Navigate to the [EC2 console](https://console.aws.amazon.com/ec2/) to view the deployed systems. Click on **Instances** in the left-hand menu. The four instances created for this lab will be prefixed with 'Prod-' in the **Name** column.

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.1.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}