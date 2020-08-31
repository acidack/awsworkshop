+++
title = "2. AWS Systems Manager: Operations as Code"
weight = 12
+++
## Operations as Code with Systems Manager
---
In a traditional environment you would have to set up the systems and software to perform administration activities. You would require a server to execute your scripts. You would need to manage authentication credentials across all of your systems.

_Operations as code_ reduces the resources, time, risk, and complexity of performing operations tasks and ensures consistent execution. You can take operations as code and automate operations activities by using scheduling and event triggers. Through integration at the infrastructure level you avoid "swivel chair" processes that require multiple interfaces and systems to complete a single operations activity.

### Setting up Systems Manager to capture tagged targets
---
1. Open the **Systems Manager** console.
2. In the  [Systems Manager](https://console.aws.amazon.com/systems-manager/) console select **Quick Setup** from the left-hand navigation menu.
3. Click on the **Edit all** at the top right corner of the console. Scroll down and change **Targets** to **Specify instance tags**.
   * Under **Tags** specify `Environment` for the **key** and `SSMPatchLab` for the **value**.

  {{% notice tip %}}
  You can constrain managed instances to those with specific tags, such as Environment or Workload.  
  You can also manually select specific instances for management, though not recommended for your environment as it scales.  
  {{% /notice %}}

4. Select the **Reset** button at the bottom of the page.  

  ![](/images/lab_3b_quick-setup-button.png)

#### *KEY CONCEPT:* What did we learn?
* The requirements and basic functionality of AWS Systems Manager and the SSM Agent
* How to configure Systems Manager and your instances so that Systems Manager can manage them


---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab2.2.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}