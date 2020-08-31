+++
title = "4. Use Port Forwarding For Web Redirection"
date = 2019-11-18T17:11:28+11:00
weight =14
+++

Session Manager Port Forwarding feature allows you to tunnel data from remote port on instance to a local port on client machine. This enables web redirection for user without opening inbound ports. You can use this feature using AWS CLI which requires you to install session-manager-plugin on client machine. It uses public SSM document **AWS-StartPortForwardingSession** that allows users to provide local and remote port numbers to enable port forwarding.

**Review SSM Document**

1.  Under Shared Resources in the AWS Systems Manager navigation menu, browse to the [Documents console](https://ap-southeast-2.console.aws.amazon.com/systems-manager/documents/AWS-StartPortForwardingSession/content?region=ap-southeast-2) and review contents of **AWS-StartPortForwardingSession** document.
2.  Note that **Session Type** is **Port** and default value for portNumber is **80**.

![](/images/porta.png)

**Install Apache HTTP Server on EC2 instance**

1.  Under Instances & Nodes in the AWS Systems Manager navigation menu, browse to the [Session Manager console](https://ap-southeast-2.console.aws.amazon.com/systems-manager/session-manager/start-session?region=ap-southeast-2) and start a session to linux instance **session-manager-linux-stage**.
2.  Type command below

        sudo yum -y install httpd; sudo systemctl enable httpd; sudo systemctl start httpd

    You should see an output as shown below.
    ![](/images/portb.png)


3.  Verify apache http server(httpd) is running on port 80 by running command 

        sudo netstat -atnp | grep -i httpd
    
    You should see an output as shown below. ![](/images/portc.png)


**Start Port Redirection**

1.  Browse to the [EC2 Console](https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#Instances:tag:Name=session-manager-linux-stage;sort=instanceId) and note instance-id for instance **session-manager-linux-stage**.

![](/images/portd.png)


2.  Browse to the [AWS Cloud9 IDE](https://ap-southeast-2.console.aws.amazon.com/cloud9/) and type below command in the console after replacing with appropriate instance ID to start a session to **session-manager-linux-stage** instance.

        aws ssm start-session --target <instance-id> --document-name AWS-StartPortForwardingSession --parameters "localPortNumber=8080"

3.  You should see a message indicating port 8080 has been opened for this session. 

   ![](/images/porte.png)


4.  Within Cloud9 to preview a web page, select **Preview** from the menu option and **Preview Running Application** as shown below. You should be able to access Apache http server home page which is running on port 443 on remote instance **session-manager-linux-stage**. ![](/images/portfw2.png)

5.  Press Control+C on terminal to terminate the session. ![](/images/portfw3.png)

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab1.4.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}