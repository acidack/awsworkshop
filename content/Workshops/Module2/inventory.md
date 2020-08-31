+++
title = "3. AWS Systems Manager: Inventory"
weight = 13
+++

## Inventory with Systems Manager
---
{{% notice info %}}
If you are using an AWS Event Engine account you might see an error at the top of your screen when accessing features with resource group integration.
These errors can safely be ignored, they will not impact your ability to perform any of the steps in the lab.
{{% /notice %}}
![](/images/lab_4_potential-ee-error.png)

---
#### *KEY CONCEPT:* AWS Systems Manager - Inventory
AWS Systems Manager collects information about your instances and the software installed on them, helping you to understand your system configurations and installed applications.  
You can collect data about applications, files, network configurations, Windows services, registries, server roles, updates, and any other system properties.  
The gathered data enables you to manage application assets, track licenses, monitor file integrity, discover applications not installed by a traditional installer, and more.  

### Using Systems Manager Inventory to Track Your Instances
---
1. Under **Instances & Nodes** in the AWS Systems Manager navigation menu, choose **Inventory**.
    * Scroll down in the window to the **Corresponding managed instances** section. Inventory displays the instance data provided by the EC2 service.
    * Click on the **InstanceID** of one of your systems that has the prefix **Prod-** in the **Name** column.
    * Examine each of the available tabs of data under the **Instance ID** heading (Description/Inventory/Tags/Associations/Patch/Configuration Compliance).  Note that **Inventory** is currently empty, as it is unconfigured.

  ![](/images/lab_4a_managed-instance-options.png)

2. Inventory collection must now be specifically configured and the data types to be collected need to be specified:
    * Choose **Inventory** from the left-hand menu.
    * Click on the **Setup Inventory** button at the top right corner of the window.

  ![](/images/lab_4a_ssm-inventory-setup.png)

3. In the **Setup Inventory** screen, define targets for inventory:
    * Under **Specify targets by**, select **Specifying a tag**
    * Under **Tags** specify `Environment` for the key and `SSMPatchLab` for the value

{{% notice tip %}}
* You could select all managed instances in an account, ensuring that all managed instances would be inventoried.  
* You can constrain inventoried instances to those with specific tags, such as Environment or Workload.  
* You can also manually select specific instances for inventory, although this is not recommended for your environment as it scales.  
{{% /notice %}}

![](/images/lab_4a_ssm-inventory-setup-targets.png)


4. You can **Schedule** the frequency with which inventory is collected. The default and minimum period is **30 minutes**.
5. Under **Parameters** you can filter what information to collect with the inventory process - review the options and keep the defaults.
6. Click on the **Setup Inventory** button at the bottom right of the page (it can take up to 10 minutes to deploy a new inventory policy to an instance).

![](/images/lab_4a_ssm-inventory-setup-button.png)

{{%notice tip %}}
You can create multiple Inventory specifications. They will each be stored as **associations** within **Systems Manager State Manager**.
{{% /notice %}}

**Congratulations!  You have now created an inventory specification and associated it with your lab instances.**

---
### What did we learn?
 * The methods to setup AWS Systems Manager to collect inventory from your instances
 * The options for scheduling inventory collection on your instances
 * How to check compliance for inventory collection across your environment

---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.3.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}