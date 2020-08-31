+++
title = "5. AWS Systems Manager: Patch Manager"
weight = 15
+++

## Patch Management with AWS Systems Manager
---
#### *KEY CONCEPT:* Systems Manager - Patch Manager

AWS Systems Manager [Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-patch.html) automates the process of patching managed instances with security related updates.

{{% notice tip %}}
For Linux-based instances, you can also install patches for non-security updates.
{{% /notice %}}

You can patch fleets of Amazon EC2 instances and/or your on-premises servers and virtual machines (VMs) by Operating System type. This includes supported versions of **Windows, Ubuntu Server, Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES), Oracle Linux, CentOS, Debian, Raspbian, Amazon Linux and Amazon Linux 2**. You can scan instances to see a report of missing patches, or you can scan and automatically install all missing patches. You can target instances for patching individually, or in large groups by using Amazon EC2 tags.

{{% notice warning %}}
* **AWS does not test patches for Windows or Linux before making them available via Patch Manager - by default the patches are provided from the patch repositories maintained by the OS vendors**.
* **If any updates are installed by Patch Manager the patched instance is rebooted**.
* **Always test patches thoroughly before deploying to production environments**.
{{% /notice %}}

#### *KEY CONCEPT:* Patch Baselines

Patch Manager uses **patch baselines**, which include rules for auto-approving patches within days of their release, as well as a list of approved and rejected patches. Later in this lab we will schedule patching to occur on a regular basis using a Systems Manager **Maintenance Window** task. Patch Manager integrates with AWS Identity and Access Management (IAM), AWS CloudTrail, and Amazon CloudWatch Events to provide a secure patching experience that includes event notifications and the ability to audit usage.

{{% notice warning %}}
The [Operating Systems supported by Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-supported-oses.html) may vary from those supported by the SSM Agent.
{{% /notice %}}

### Create a Patch Baseline
---
1. Under **Instances and Nodes** in the **AWS Systems Manager** navigation menu on the left, choose **Patch Manager**.
2. Click the **View predefined patch baselines** button in the **Patch your instances** section at the upper right of the console.
![](/images/lab_6a_ssm-inventory-activate.png)
3. Click on the **Create patch baseline** button at the top right of the console.
![](/images/lab_6a_ssm-patch-baselines.png)
4. On the **Create patch baseline** page, in the **Provide patch baseline details** section:
    * Enter a **Name** for your custom patch baseline, such as `AmazonLinuxSecAndNonSecBaseline`.
    * Optionally enter a description, such as `Amazon Linux patch baseline including security and non-security patches`.
    * Select **Amazon Linux** from the list.
![](/images/lab_6a_ssm-create-patch-baseline.png)

5. In the **Approval rules for operating systems** section:
    * Examine the options in each of the dropdown lists and ensure that **Product**, **Classification**, and **Severity** are set to values of **All**.
    * Leave the **Auto approval** configuration item set to the selection **Approve patches after a specified number of days** and the default of **0 days**.
    * Change the value of **Compliance reporting - optional** to **Critical**.
![](/images/lab_6a_ssm-rule1.png)
    * Click on the **Add rule** button to create a second rule (it is possible to create up to 10 rules in the one patch baseline).
    * For Rule 2, ensure that **Product**, **Classification**, and **Severity** are set to values of **All**.
    * Change the value of **Compliance reporting - optional** to **Medium**.
    * Check the box under **Include nonsecurity updates** to include all Amazon Linux updates when patching (not just for vulnerabilities).
![](/images/lab_6a_ssm-rule2.png)

{{% notice info %}}
If an approved patch is reported as missing, the option you choose in **Compliance reporting**, such as `Critical` or `Medium`, determines the severity of the compliance violation reported in System Manager **Compliance**.
{{% /notice %}}

6. Expand the **Patch exceptions** section by clicking on the small arrow. In the **Rejected patches - optional** text box, enter `system-release.*` This will [reject patches](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-approved-rejected-package-name-formats.html) to new Amazon Linux releases that may advance you beyond the [Patch Manager supported operating systems](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-supported-oses.html) prior to your testing of new releases.
![](/images/lab_6a_ssm-patch-baseline-exception.png)
7. For Linux operating systems, you can optionally define an alternative [patch source]( https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-alt-source-repository.html) repository.
8. You can also add tags to a patch baseline in the **Manage tags** section (we will not do so in this lab).
9. Click on the **Create patch baseline** button at the bottom right of the console.
![](/images/lab_6a_ssm-patch-source.png)
10. Now click on the **View details** button at the top right of the console (in the green header bar) to go to the **Patch Baselines** page, to view the configuration of your custom patch baseline.
![](/images/lab_6a_ssm-patch-viewcustom.png)

#### *KEY CONCEPT:* Patch Groups

A [patch group](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-patch-patchgroups.html) is an optional method to organize instances for patching. For example, you can create patch groups, for example by Operating Systems (Linux or Windows), different environments (Development, Test, Production), or different server functions (web servers, file servers, databases). Patch groups can help you avoid deploying patches to the wrong set of instances. They can also help you avoid deploying patches before they have been adequately tested.

You create a patch group by using Amazon EC2 tags. Unlike other tagging scenarios across Systems Manager, a patch group must be defined with the tag key: `Patch Group` (tag keys are case sensitive). You can specify any value (for example, `web servers`) but the key must be `Patch Group`.

{{% notice info %}}
An instance can only be in one patch group at a time.
{{% /notice %}}

After you create a patch group and tag instances, you can register the patch group with a patch baseline. By registering the patch group with a patch baseline, you ensure that the correct patches are installed during the patching execution. When the system applies a patch baseline to an instance, the service checks if a patch group is defined for the instance.  

* If the instance is assigned to a patch group, the system checks to see which patch baseline is registered to that group.
* If a patch baseline is found for that group, the system applies that patch baseline.
* If an instance isn't assigned to a patch group, the system automatically uses the currently configured default patch baseline.

### Assign a Patch Group
---
1. Click on the  **Actions** in the top right of the console and select **Modify patch groups**.
![](/images/lab_6b_ssm-patch-baseline-modify-patch-group.png)
2. In the **Modify patch groups** window in the text field for **Patch groups**, type `Critical`, click on the **Add** button, then click on the **Close** button to be returned to the **Patch Baseline** details screen.
![](/images/lab_6b_ssm-modify-patch-groups.png)

#### *KEY CONCEPT:* AWS Systems Manager - Documents

An [AWS Systems Manager document](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html) defines the actions that Systems Manager performs on your managed instances. Systems Manager includes many pre-configured documents that you can use by specifying parameters at runtime, including 'AWS-RunPatchBaseline'. These documents use JavaScript Object Notation (JSON) or YAML, and they include steps and parameters that you specify.

All AWS-provided *Automation* and *Run Command* documents can be viewed in AWS Systems Manager **Documents**. You can [create your own documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/create-ssm-doc.html) or launch existing scripts using provided documents to implement custom operations as code activities.

#### *KEY CONCEPT:* AWS-RunPatchBaseline Document

[AWS-RunPatchBaseline](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-ssm-documents.html#patch-manager-ssm-documents-recommended-AWS-RunPatchBaseline) is a command document that enables you to control patch approvals using patch baselines. It reports patch compliance information that you can view using the Systems Manager **Compliance** tools. For example, you can view which instances are missing patches and what those patches are.

For Linux operating systems, compliance information is provided for patches from both the default source repository configured on an instance and from any alternative source repositories you specify in a custom patch baseline. *AWS-RunPatchBaseline* supports both Windows and Linux operating systems.

### Examine AWS-RunPatchBaseline in Documents
---
To examine AWS-RunPatchBaseline in Documents:

1. In the AWS Systems Manager navigation menu under **Shared Resources**, choose **Documents**.
2. Click in the **search box**, select **Document name prefix**, and then **Equals**.
3. Type `AWS-Run` into the text field and press _Enter_ on your keyboard to start the search (this is case-sensitive):
![](/images/lab_6c_patchbaselinefilter.png)
4. Click on the document named **AWS-RunPatchBaseline** (click on the blue text of the document name) to view the details:
![](/images/lab_6c_AWSRunPatchBaseline-Doc-Description.png)
5. Click on the **Content** tab to view the configuration of the document:
![](/images/lab_6c_AWSRunPatchBaseline-Doc-Content.png)

#### *KEY CONCEPT:* AWS Systems Manager - Run Command

[AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) lets you remotely and securely manage the configuration of your managed instances.  *Run Command* enables you to automate common administrative tasks and perform ad hoc configuration changes at scale. You can use Run Command from the AWS Management Console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs.

### Scan Your Instances with AWS-RunPatchBaseline via Run Command
---
1. Under **Instances and Nodes** in the AWS Systems Manager navigation menu on the left, choose **Run Command**. In the Run Command dashboard, you can view previously executed commands in the **Command history** tab, including the execution of AWS-RefreshAssociation, which was performed when you set up inventory.
2. Choose **Run Command** in the top right of the window.
3. In the **Run a command** window, under **Command document**:
    * Choose the search icon and select `Platform types`, and then choose `Linux` to display all the available commands that can be applied to Linux instances.
    * Select **AWS-RunPatchBaseline** by clicking in the small circle to the left of its name in the list.
![](/images/lab_6d_ssm-AWSRunPatchBaseline.png)
4. Scroll down to **Command parameters**. In the **Command parameters** section, leave the **Operation** value on the default choice of **Scan**.
5. In the **Targets** section:
   * Choose **Specify instance tags** to reveal the **Specify Instance Tags** fields sub-section.
   * Under **Enter a tag key**, enter `Environment`, and under **Enter a tag value**, enter `SSMPatchLab` and click on the **Add** button.
![](/images/lab_6d_ssm-AWSRunPatchBaseline-scan-targets.png)
   * Optionally the remaining *Run Command* features enable you to:  
      * Specify **Rate control**, limiting **Concurrency** to a specific number of targets or a calculated percentage of systems, or to specify an **Error threshold** by count or percentage of systems after which the command execution will end.
      * Specify **Output options** to record the entire output to a pre-configured **S3 bucket** and optional **S3 key prefix**.
      **Note** Only the last 2500 characters of a command document's output are displayed in the console.
      * Specify **SNS notifications** to a specified **SNS Topic** on all events or on a specific event type for either the entire command or on a per-instance basis. This requires Amazon SNS to be pre-configured.
      * View the command as it would appear if executed within the AWS Command Line Interface.
![](/images/lab_6d_ssm-AWSRunPatchBaseline-cli.png)
6. Click on the **Run** button in the bottom right of the console to execute the command and return to its details page.
7. Scroll down to **Targets and outputs** to view the status of the individual targets that were selected through your tag key and value pair. Refresh the status list by clicking on the circular arrow at the top right of the console.
8. Click on one of the blue names of an **Instance ID** in the targets list to view the **Output** from command execution on that instance.
9.  Click on the small arrow next to **Step 1 - Output** to view the first 2500 characters of the command output from Step 1 of the command. CLick on the small arrow next to **Step 1 - Output** again to conceal it.
10.  Click on the small arrow next to **Step 2 - Output** to view the first 2500 characters of the command output from Step 2 of the command.  The execution step for **PatchWindows** was skipped as it did not apply to your Amazon Linux instance. Click on the small arrow next to **Step 2 - Output** again to conceal it.

### Review Initial Patch Compliance
---
1. Under **Instances & Nodes** in the AWS Systems Manager navigation menu on the left, choose **Compliance**.
2. On the **Compliance** page in the **Compliance resources summary**, you will now see that there are 4 systems that have critical severity compliance issues. In the **Resources** list, you will see the individual compliance status and details for resources:
![](/images/lab_6e_ssm-patch-compliance.png)

### Patch Your Instances with AWS-RunPatchBaseline via Run Command
---
1. Under **Instances and Nodes** in the AWS Systems Manager navigation menu, choose **Run Command**.
2. Click on the **Run Command** button in the top right of the console.
3. In the **Run a command** window, under **Command document**:
    * Click in the search text field, select `Platform types`, and then choose `Linux` to display all the available commands that can be applied to Linux instances.
    * Select **AWS-RunPatchBaseline** by clicking in the small circle to the left of its name in the list.
4. In the **Command parameters** section, change the **Operation** value in the dropdown menu to **Install**.
5. In the **Targets** section:
    * choose **Specify instance tags**.
    * Under **tag key**, enter `Environment` and under **tag value** enter `SSMPatchLab`, then click on **Add**.
![](/images/lab_6f_ssm-AWSRunPatchBaseline-install-targets.png)

  {{% notice tip %}}
  You could have chosen **Manually selecting instances** and used the check box at the top of the list to select all instances displayed, or selected them individually.
  If there are multiple pages of instances, individual selections must be made on each page.
  {{% /notice %}}

6. Click on the small arrow next to **Rate control** to expand it:
    * For **Concurrency**, ensure that **targets** is selected and specify the value as `1`.
{{% notice tip %}}
Limiting concurrency will stagger the application of patches and the reboot cycle, however, to ensure that your instances are not rebooting at the same time, create separate tags to define target groups and schedule the application of patches at separate times.
{{% /notice %}}

    * For **Error threshold**, ensure that **error** is selected and specify the value as `1`.
7. In **Output options**, untick the selection box next to **Enable writing to an Amazon S3 bucket**.
8. Click on the **Run** button at the bottom right of the console to execute the command and to go to its details page.
![](/images/lab_6d_ssm-AWSRunPatchBaseline-rate-output.png)

1. Refresh the page by clicking on the circular arrow at the top right of the console to view updated status. This step may take up to 15 minutes, so do not wait for all four targets to complete.  Take a refreshment break at this point for a few minutes and then come back to the lab exercise.
![](/images/lab_6f_ssm-AWSRunPatchBaseline-status.png)

{{% notice warning %}}
Remember, if any updates are installed by Patch Manager, the patched instance may be rebooted.
{{% /notice %}}

### Review Patch Compliance After Patching
---
1. Under **Instances & Nodes** in the AWS Systems Manager navigation menu, choose **Compliance**.
2. The **Compliance resources summary** will now show that patched systems will now have satisfied critical severity patch compliance and will now be counted as **compliant resources** in the **Patch** row.
![](/images/lab_6g_ssm-patch-compliance-success.png)
3. Click on the blue number next to the green tick in the **Patch** row to see the newly-patched instances listed further down in the console:
![](/images/lab_6g_ssm-patch-compliance-newlypatched.png)


#### *KEY CONCEPT:* What did we learn?
* The basic concepts and functionality of the AWS Systems Manager *Patch Manager* feature
* The basic concepts and functionality of the Systems Manager *Run Command* feature
* How to setup custom *Patch Baselines* for *Patch Manager* that are aligned with your business requirements
* How to use the *Run Command* to scan for missing patches and update the Patch compliance status for reporting
* How to use the *Run Command* to install missing patches using configuration options that ensure patching will not impact availability.

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.5.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}