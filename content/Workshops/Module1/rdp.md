+++
title = "6. Enable RDP Through Session Manager"
date = 2019-11-18T17:11:28+11:00
weight =16
+++
You can tunnel Remote Desktop Protocol (RDP) using Port Forwarding feature of Session Manager to get access to remote Windows instance. This can be achieved without opening inbound port 3389 (default RDP port) on remote instance.

{{% notice warning %}}
You will require AWS CLI and RDP client installed on your local machine for this step and adequate access over the Internet. If you do not have required access, you may choose to skip this step. 
{{% /notice %}}

### Prerequisites 
#### Install and configure AWS CLI and Session Manager CLI Plugin into your local machine

1. Install the latest version of [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
2. Install the [Session Manager plugin for the AWS CLI](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)
3. Browse to  [Users under the IAM console](https://console.aws.amazon.com/iam/home?region=ap-southeast-2#/users/session-manager-demo-user?section=security_credentials) and select **session-manager-demo-user** from the Users  ![](/images/rdpa.png)
4. Go into the **Security credentials** tab and select **Create access key**. Note down the **Access key ID** and **Secret Access key** for step 5.  ![](/images/rdpb.png)
5. [Configure the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration) via the command below and enter your credentials with region as **ap-southeast-2**. 
        

        aws configure

    ![](/images/awsconfigexample.png)

#### Install RDP Client on your local machine

1.  If you do not already have the RDP Client, download and install from below
    - [Windows] Windows includes an RDP client by default. To verify, type **mstsc** at a Command Prompt window. If your computer doesn't recognize this command, see the [Windows home page](https://windows.microsoft.com/) and search for the download for the Microsoft Remote Desktop app.

    - [Mac OS X] Download the Microsoft Remote Desktop app from the Mac App Store.

    - [Linux] Use [Remmina](https://remmina.org/)
  
#### Create a Windows OS user

1.  Under Instances & Nodes in the AWS Systems Manager navigation menu, browse to the  [Session Manager console](https://ap-southeast-2.console.aws.amazon.com/systems-manager/session-manager/start-session?region=ap-southeast-2) and start a session to windows instance **session-manager-windows-stage**.
2.  Type the following commands to create a new user:
    -   Input password as a secure string. Enter below command which will prompt you for a password, then type a strong password and enter: 
 
                $Password = Read-Host -AsSecureString

    -   Create a local user: 
  
                New-LocalUser "User01" -Password $Password

    -   Add user to Remote Desktop Users group: 

                Add-LocalGroupMember -Group "Remote Desktop Users" -Member "User01"

    ![](/images/rdp21.png)
3.  Click **Terminate** to terminate the session or enter **exit** and select close.


#### RDP to EC2 Instance

1.  Browse to the [EC2 Console](https://ap-southeast-2.console.aws.amazon.com/ec2/home?region=ap-southeast-2#Instances:search=session-manager-windows-stage;sort=instanceId) and note instance-id for instance **session-manager-windows-stage**.

2.  Open a terminal on your local machine and type below command to start a session to instance **session-manager-windows-stage** instance.

        aws ssm start-session --target <instance-id> --document-name AWS-StartPortForwardingSession --parameters "localPortNumber=55678,portNumber=3389"

3.  You should see a message indicating port **55678** has been opened for this session. ![](/images/rdp1.png)

4.  Open Microsoft Remote Desktop client and add a new remote desktop with below information.
    -   PC Name: `localhost:55678`
    -   User Account: Provide user name and password created in earlier step
    -   Connection/Friendly Name: **Session Manager RDP** ![](/images/rdp31.png)

5.  Using Microsoft Remote Desktop, open remote desktop connection **Session Manager RDP** configured earlier for localhost:55678. You should be now connected and able to work on remote instance over RDP. ![](/images/rdp4.png)

6.  Press Control+C on terminal to terminate the session. ![](/images/rdp5.png)

7.  **Congratulations! You have now completed this workshop!** 

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab1.6.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}