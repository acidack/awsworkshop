+++
title = "5. Enable SSH Through Session Manager"
date = 2019-11-18T17:11:28+11:00
weight =15
+++

Session Manager can be configured to connect to remote instance using Secure Shell(SSH) without opening inbound port or maintaining bastion host. You can also copy files between local and remote machine using Secure Copy Protocol(SCP). This feature uses public SSM document AWS-StartSSHSession.

**Review SSM Document**

1.  Browse to the [Systems Manager Document console](https://ap-southeast-2.console.aws.amazon.com/systems-manager/documents/AWS-StartSSHSession/content?region=ap-southeast-2) and review content of **AWS-StartSSHSession** document.
2.  Note that **Session Type** is **Port** and **Default Port** provided is **22**.

**Launch an EC2 Instance**

1.  Browse to the [EC2 Console](https://ap-southeast-2.console.aws.amazon.com/ec2/home?region=ap-southeast-2#Instances:sort=instanceId) and click **Launch Instance**.
2.  Choose below configurations for the instance.
    -   AMI: Amazon Linux 2 
    -   Instance Type: t2.micro
    -   Network: smdemo-vpc
    -   IAM role: session-manager-demo-default
    -   Storage: leave default value
    -   Tag: Key -> Name, Value -> session-manager-demo-linux-ssh
    -   Security Group: Remove SSH rule from the launch wizard Security Group
    -   Key Pair: Select **MyKeyPair**

**Configure ssh proxy command**

1.  Browse to the [AWS Cloud9 IDE](https://ap-southeast-2.console.aws.amazon.com/cloud9/) and type the following command 

        nano ~/.ssh/config

2. Add below proxy command and save the file (to save, type CTRL-X, type Y and hit Enter).

         host i-* mi-*
         ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
3. Run the following command to provide adequate permissions to the file. 

        chmod 600 ~/.ssh/config
   
**SSH to EC2 instance**

1.  Browse to the [EC2 Console](https://ap-southeast-2.console.aws.amazon.com/ec2/home?region=ap-southeast-2#Instances:search=session-manager-demo-linux-ssh;sort=instanceId) and note instance-id for instance **session-manager-demo-linux-ssh**.
2.  Type command `ssh -i MyKeyPair.pem ec2-user@[INSTANCE-ID]` to ssh to the instance **session-manager-demo-linux-ssh** using PEM file generated during instance launch. Type `yes` and press enter to confirm. 
3.  Congratulations! You should be successfully logged in to the instance and see output as below. ![](/images/ssh1.png)
4.  Type `exit` on terminal to terminate the session.

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab1.5.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}