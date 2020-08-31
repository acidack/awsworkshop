+++
title = "4. AWS Systems Manager: State Manager"
weight = 14
+++

## State Manager with Systems Manager
---
#### *KEY CONCEPT:* AWS Systems Manager - State Manager

In State Manager, an [association](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-associations.html) is the result of binding configuration information that defines the state you want your instances to be in to the instances themselves. This information specifies when and how you want instance-related operations to run that ensure your Amazon EC2 and hybrid infrastructure is in an intended or consistent state.

An association defines the state you want to apply to a set of targets. An association includes three mandatory components and one optional set of components:  

  * A document that defines the state
  * Target(s)
  * A schedule
  * (Optional) Runtime parameters.

When you performed the **Setup Inventory** actions in the previous step, you created an *association* in State Manager.

#### *KEY CONCEPT:* AWS Systems Manager - Compliance

You can use AWS Systems Manager Configuration [Compliance](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-compliance.html) to scan your fleet of managed instances for patch compliance and configuration inconsistencies.
You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant.
By default, Configuration Compliance displays compliance data about Systems Manager Patch Manager patching and **Systems Manager State Manager** associations.
You can also customize the service and create your own compliance types based on your IT or business requirements.
You can also port data to **Amazon Athena** and **Amazon QuickSight** to generate fleet-wide reports.

### Review Association Status
---
1. Under **Instances & Nodes** in the navigation menu on the left, select **State Manager** (At this point, the **Status** may show that the inventory activity has not yet completed - if not, refresh the screen until all associations have a status of **success**).
    * Click on the blue **Association id** that is the result of your **Setup Inventory** action (the **Association Name** for the correct association id you want to view for this lab is **Inventory-Association**).
  ![](/images/lab_5a_ssm-inventory-assoc-status.png)  
    * Examine each of the available tabs of data under the **Association ID** heading.
  ![](/images/lab_5a_ssm-inventory-state-manager-assoc.png)
    * Click on the **Edit** button at the top right of the console.
    * Enter a name under **Name - optional** to provide a more user friendly label to the association, such as `InventoryAllInstances` (white space is not permitted in an _Association Name_).
    * Scroll to the bottom of the screen and click on **Save Changes**.

  ![](/images/lab_5a_ssm-assoc-friendly-name.png)

_Inventory_ is accomplished through the following:
  * The activities defined in the **AWS-GatherSoftwareInventory** command document.
  * The parameters provided in the **Parameters** section are passed to the document at execution.
  * The targets are defined in the **Targets** section.
  {{% notice tip %}}
  In this example there is a single target, the wild-card. The wild-card matches _all_ instances making them _all_ targets.
  {{% /notice %}}
  * The schedule for this activity is defined under **Specify schedule** and **Specify with** to use a CRON/Rate expression on a 30 minute interval.
  * There is the option to specify **Output options**.
  **Note:** If you change the command document, the **Parameters** section will change to be appropriate to the new command document.

2. Navigate to **Managed Instances** under **Instances and Nodes** in the navigation menu on the left. An **Association Status** has been established for the inventoried instances under management.
3. Click on one of the blue **Instance ID** links (for one of the instances that has a 'Prod-' name prefix) to go to the details of the instance. The **Inventory** tab is now populated; you can track associations and their last activity under the **Associations** tab.

![](/images/lab_5a_instance-inventory-details.png)

4. Navigate to **Compliance** under **Instances & Nodes** in the navigation menu on the left. Here you can view the overall compliance status of your managed instances in the **Compliance resources summary**
![](/images/lab_5a_ssm-inventory-compliance.png)
5. Navigate to **Inventory** under **Instances & Nodes** in the navigation menu on the left, to view individual compliance status of systems in the **Corresponding managed instances** section.
![](/images/lab_5a_ssm-inventory-managed-instances.png)

{{% notice info %}}
The inventory collection activity can take up to 10 minutes to complete. Whilst waiting for the inventory activity to complete, you can continue with the next section.
{{% /notice %}}

#### *KEY CONCEPT:* What did we learn?
* The functionality of the Systems Manager State Manager feature
* The types of associations that State Manager can create
* How to setup State Manager associations for your environment
* How to view and edit State Manager associations

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.4.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}