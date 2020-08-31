+++
title = "3. Configure Systems Manager Session Manager"
date = 2019-11-18T17:11:28+11:00
weight =12
chapter = false

+++

As we observed during our initial evaluation, our activity within a session is not yet being logged. In this step, we are going to configure Session Manager to store session log data in a specified Amazon S3 bucket for auditing purposes. The default option is for logs to be sent to an encrypted S3 bucket. Encryption is performed using the key specified for the bucket.

**Create a Custom Policy for Amazon S3 Bucket Access**

Creating a custom policy for Amazon S3 access is required only if you are using a VPC endpoint or using an S3 bucket of your own in your Systems Manager operations.

1.  Browse to the [CloudFormation console](https://ap-southeast-2.console.aws.amazon.com/cloudformation/), and select the stack starting with **mod-** and Description **Session Manager workshop**. Select the output tab and note down the S3 bucket name with the key value of **Logging Bucket**. This bucket will follow the naming convention session-manager-demo-[ACCOUNT_NUMBER]-ap-southeast-2 as below. 
    ![](/images/cfnsessionoutput.png)

2. Open the [IAM console](https://console.aws.amazon.com/iam/)

3.  In the navigation pane, choose **Policies**, and then choose **Create policy**. ![](/images/iam1.png)

4.  Choose the **JSON** tab, and replace the default text with the following:

        {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:PutObjectAcl", 
                    "s3:GetEncryptionConfiguration" 
                ],
                "Resource": [
                    "arn:aws:s3:::my-bucket-name/*",
                    "arn:aws:s3:::my-bucket-name" 
                ]
            }
        ]
        }


    Replace `my-bucket-name` with the bucket name from step 1.

5.  Choose **Review policy**. ![](/images/iam2.png)

6.  For **Name**, enter a name to identify this policy, such as **SSMInstanceProfileS3Policy** or another name that you prefer. ![](/images/iam3.png)

7.  Choose **Create policy**.

**Attaching Policy to Instance Profile Role**

1.  After the policy has been created, open the [IAM console](https://console.aws.amazon.com/iam/home#/roles/session-manager-demo-default) and head into **Roles** in the navigation menu to the left and search for **session-manager-demo-default**. ![](/images/iam4.png)
2.  Then, click on **Attach policies**.
3.  In the **Filter**, enter **SSMInstanceProfileS3Policy** as the policy to select.
4.  Select the policy and click on **Attach policy**.

**Enable Session Logging for Session Manager**

Once the policy has been created and associated with our Instance Profile, we will configure session logging to the S3 bucket we created using CloudFormation.

1.  Under Instances & Nodes in the AWS Systems Manager navigation menu, browse to  **Session Manager** and select **Preferences** tab and [**Edit** option](https://ap-southeast-2.console.aws.amazon.com/systems-manager/session-manager/edit-preferences?region=ap-southeast-2)
2.  Under the **Write session output to an Amazon S3 bucket** heading, select **S3 bucket**.
3.  Leave **Encrypt log data** selected. In the **S3 bucket name** field, select the bucket you noted down at the start of this step. In the **S3 key prefix** field, enter **Session-Manager**. Click **Save** ![](/images/sessionmgr9.png)
4.  Select the [Sessions tab](https://ap-southeast-2.console.aws.amazon.com/systems-manager/session-manager/edit-preferences?region=ap-southeast-2#), click **Start Session**, select a Linux instance and click **Start session**. ![](/images/sessionmgr10.png)
5.  Execute some basic commands to demonstrate session logging is working as expected:
    -   `ps -ef | grep -i ssm`
    -   `sudo ls -l ../etc`
    -   `whoami`
    -   `exit` terminate the session.

6.  Browse to the [Session History tab](https://ap-southeast-2.console.aws.amazon.com/systems-manager/session-manager/sessions-history?region=ap-southeast-2) and locate our last session. Wait for session **Status** to change from **Terminating** to **Terminated**. In the **Output location** column, click **Amazon S3** to view the session log. ![](/images/sessionmgr11.png)
7.  Observe the data captured in the session log includes all input and output of the commands we entered.

{{% notice note %}}
**Note:** The S3 bucket created for you is configured to use [S3 Default Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html) with AWS S3-managed keys (SSE-S3). All session log data will be encrypted by default but you can also choose to use your own KMS Customer Master Key (SSE-KMS).
{{% /notice %}}


### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab1.3.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}