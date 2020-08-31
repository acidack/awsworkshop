+++
title = "7. Host Layer - Verify"
date = 2019-11-18T17:11:28+11:00
weight =8
+++


### Identifying and Remediating Host Vulnerabilities - Host Layer Round - Verify Phase 

Now that you have remediated the environment, you will again use Amazon Inspector to assess the environment again to see how the patching affected the overall security posture of the environment. You will first run an Inspector assessment. While the assessment is running, you will explore some other AWS capabilities. Lastly, you will return to Inspector to see the results of the new assessment to the effects of the patching that you did with Systems Manager Patch Manager.

Run another Inspector assessment
--------------------------------

1.  Go to the Amazon Inspector console, click **Assessment templates** on the menu.

2.  Locate the template that you created during the Assess Phase and check the box at the left end of that row.

3.  Click **Run**. This will launch another assessment run.

4.  The assessment run will take 15 minutes to complete.

Explore AWS Systems Manager Maintenance Windows
-----------------------------------------------

While Inspector is running, you will learn about AWS Systems Manager Maintenance Windows. AWS Systems Manager Maintenance Windows let you define a schedule for things like patching an operating system. So, rather than applying patches once as you did earlier, you can set up a maintenance window to apply patches on an ongoing basis. Each maintenance window has a schedule (when the maintenace is to occur), a set of registered targets for the maintance (in this case, the Amazon EC2 instances that are part of this workshop), and a set of registered tasks (in this case, the patching operation).

### Create the maintenance window

1.  Go to the Systems Manager console window and select **Maintenance Windows**.

2.  Click **Create maintance window**. Use the values in the table below. You can leave all other defaults in place. **Note the future start date of the window.** This is done to avoid interfering with the Inspector scan that is currently running.

| Field name | Field Value | 
|----------|----------|
| Name |   pww\_mw |
| Specify with |  Cron schedule builder
| Window starts      |                    Every 12 hours |
| Duration    |                     1 hour |
| Stop initiating tasks    |              0 hours |
| Window start date       |               12/01/2099 |
| Schedule time zone    |                 *Select your timezone* |

Your screen should look similar to the figure below.
![verify create maintnance window](/images/verify-mwcreate.png)

3.  Click **Create maintenance window** to save the maintenance window.

### Register the Maintenance Window target

1.  In the list of maintenance windows, click on the id of the window corresponding to the maintance window you just created. The link begins with a *mw-* prefix.

2.  Click the **Actions** button and select the **Register targets** menu item. Use the values in the table below. Leave all other fields at their default values.

| Field name | Field Value | 
|----------|----------|
| Target name | pww_targets |
| Specify instance tags | Select the radio button |
| Tag key | aws:cloudformation:stack-name |
| Tag value | *the stack name* (should start with "mod-" with the description of WAF Workshop Demo in the CloudFormation console) |

Make sure you click **Add** to add the tag. Your screen should look similar to the figure below.

![verify register target for maintnance window](/images/verify-mwregtarget.png) 

1.  Click **Register target** to register the target to the maintenance window based on the information you entered.

### Register the Maintenance Window task

1.  Click **Maintenance windows** on the left menu.

2.  In the list of maintenance windows, click on the id of the window corresponding to the maintance window you just created. The link begins with a *mw-* prefix.

3.  Click the **Actions** button and select the **Register Run command task** menu item. Use the values in the table below. Leave all other fields at their defualt values.

| Field name | Field Value | 
|----------|----------|
| Task name | pww\_task   |
| Command document | AWS-RunPatchBaseline |
| Target by | Selecting registered target groups |
| Concurrency |  1 targets |
| Error threshold |  1 errors |
| Service role option |  Use the service-linked role for Systems Manager |  
| Operation | Install |


Your screen should look similar to the figure below.

![verify register task for maintnance window](/images/verify-mwregtask.png)

4.  Click **Register Run command task** to register the patching task to the maintenance window based on the information you entered.

You have now completed the definition of a Systems Manager Maintenance Window. The purpose of this additional task is to show you how you could implement patching on an ongoing basis in your environment.

Examine the results of the Inspector assessment
-----------------------------------------------

Now that you have explored some additional AWS capabilities, you will examine the results of the second Inspector assessment.

1.  Go to the Inspector console. Click **Assessment runs** and periodically refresh the screen. Wait until the status for the run changes to *Analysis complete*. The run will take approximately 15 minutes to complete.

2.  Compare the number of findings between the two runs. In most cases, there will be fewer findings in the newer run since patches have been applied. The change in the number findings may vary based on the age of the AMI used to launch the instances.

3.  Click the number of findings for the newest run (after the patches were installed). You will then see all of the findings that were not patched during the Remediate Phase.

4.  Take a look at the entries that were not patched. A common example of a finding is an instance is configured to allow users to log in with root credentials over SSH, without having to use a command authenticated by a public key. Why would Patch Manager not patch this or the other findings?

5.  **Congratulations! You have now completed this workshop!** 

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab3.7.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}