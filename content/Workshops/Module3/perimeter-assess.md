+++
title = "2. Perimeter Layer - Assess"
date = 2019-11-18T17:11:28+11:00
weight =3
+++

## Environment Setup

{{%expand "Click here if you're not at an AWS event or are using your own account" %}}

In order to complete these workshops, you'll need a valid, usable [AWS Account](https://aws.amazon.com/getting-started/). Use a personal account or create a new AWS account to ensure you have the necessary access and that you do not accidentally modify corporate resources. Do **not** use an AWS account from the company you work for. **We strongly recommend that you use a non-production AWS account for this workshop such as a training, sandbox or personal account. If multiple participants are sharing a single account you should use unique names for the stack set and resources created in the console.**

**Create an admin user**

If you don't already have an AWS IAM user with admin permissions, please use the following instructions to create one:

1. Browse to the [AWS IAM](https://console.aws.amazon.com/iam/) console.
2. Click **Users** on the left navigation and then click **Add User**.
3. Enter a **User Name**, check the checkbox for **AWS Management Console** access, enter a **Custom Password,** and click **Next:Permissions**.
4. Click **Attach existing policies directly**, click the checkbox next to the **AdministratorAccess**, and click **Next:review**.
5. Click **Create User**
6. Click **Dashboard** on the left navigation and use the **IAM users sign-in link** to login as the admin user you just created."

To setup the workshop environment, launch the CloudFormation stack below in the **ap-southeast-2** AWS region using the "Deploy to AWS" links below. This will automatically take you to the console to run the template. In order to complete these workshops, you'll need a valid, usable AWS Account. Use a personal account or create a new [AWS account](https://aws.amazon.com/getting-started/) to ensure you have the necessary access and that you do not accidentally modify corporate resources. Do not use an AWS account from the company you work for. **We strongly recommend that you use a non-production AWS account for this workshop such as a training, sandbox or personal account. If multiple participants are sharing a single account you should use unique names for the stack set and resources created in the console.**

| Lab | Template | Region |
|-----|----------|---------|
| Lab 3 - Protecting Workloads from the Instance to the Edge | [![](/images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build.yml) | AP Southeast 2 (Sydney) |

1. Click Next on the Specify Template section.

2. On the Specify stack details step, update the following parameters depending on how you are doing this workshop: info "Individual or an event not sponsored by AWS"

3. If you are sharing an AWS account with someone else in the same region, change the name of the stack to **pww-yourinitials**

4. Automated Scanner: **Set to false.**

5. Scanner Username: Enter **null**

6. Scanner Password: Enter **null**

7. Trusted Network CIDR: Enter a trusted IP or CIDR range you will access the site from using a web browser. You can obtain your current IP at Ifconfig.co The entry should follow CIDR notation. i.e. 10.10.10.10/32 for a single host.

8. Keep the defaults for the rest of the parameters.

9. Click **Next**

10. Click **Next** on the **Configure stack options** section.

11. Finally, acknowledge that the template will create IAM roles under Capabilities and click **Create**.

This will bring you back to the CloudFormation console. You can refresh the page to see the stack starting to create. Before moving on, make sure the stack is in a **CREATE_COMPLETE** status. 

{{% /expand%}}


{{%expand "Click here if you are at an AWS event where the Event Engine is being used" %}}


### Identify the stack that has been provisioned successfully

1. In the CloudFormation console you should see a list of stacks similar to the figure below. Select the stack with the description of "WAF Workshop Demo"

![Stack](/images/assess-cloudformation-stacks.png)

{{% /expand%}}

### Look up the Stack Outputs

1. Go to the stack outputs and look for the website URL stored in the **albEndpoint** output value. Test access to the site by right clicking and opening in a new tab. Note the URL for your site as this will be used throughout this workshop.

2. While in the stack outputs, note the **UniqueId** value. This Id value will be used to identify the posture of your site within the automated scanner  and the [associated dashboard.](https://waflabdash-ap-southeast-2.awssecworkshops.com/) 

3. While still in stack outputs, right click the link in **RedTeamHostSession** and open in a new tab. This will launch an AWS Systems Manager Session Manager to the host you will use to perform manual scans against your site URL. 

### Website Scanning Environment and Tools

In order to test your AWS WAF ruleset, this lab has been configured with two scanning capabilities; a Red Team Host where you can invoke manual scanning and an automated scanner which runs from outside your lab environment.

The scanner performs 10 basic tests designed to help simulate and mitigate common web attack vectors.

1. Canary GET - Should not be blocked
2. Canary POST - Should not be blocked
3. SQL Injection (SQLi) in Query String
4. SQL Injection (SQLi) in Cookie
5. Cross Site Scripting (XSS) in Query String
6. Cross Site Scripting (XSS) in Body
7. Inclusion in Modules
8. Cross Site Request Forgery (CSRF) Token Missing
9. Cross Site Request Forgery (CSRF) Token Invalid
10. Path Traversal

{{% notice tip %}}
These basic tests are designed to provide common examples you can use to test AWS WAF functionality. You should perform thorough analysis and testing when implementing rules into your production environments. {{% /notice %}}

### Website Scanning Environment and Tools - Manual Scanning

Once you have started a Session Manager connection to your Red Team Host, the scanner script can be invoked by typing the following command:

`` runscanner ``

![Initial Scan](/images/initial-scan-term.svg)

The scanner script will run each of the tests above and report back the following information:

- **Request:** The HTTP request command used.
- **Test Name:** The name of the test from list above.
- **Result:** The HTTP status code returned.

The logic in the scanner script color codes the response as follows:

- **Green:** 403 - Forbidden (Except for canary GET and POST tests.)
- **Red:** 200 - OK
- **Blue:** 404 - Not Found
- **Yellow:** 500 - Internal Server Error

{{% notice tip %}}

*About Scanner Tests and Colors: The color coding of the tests is provided to help to quickly assess the behavior of your WAF rules against their intended behavior. The goal is to achieve green color responses for all the tests. The purpose of the canary GET and POST requests are to ensure you have not unintentionally blocked legitimate traffic to your test site. These two tests should always return a 200 - OK response.*
{{% /notice %}}


What are the results of running the scanner script? Were the simulated malicious requests blocked? As you can see by running the script there are several vulnerabilities that need to be addressed. In the remediate phase you will configure an AWS WAF Web ACL to block these requests. When AWS WAF blocks a web request based on the conditions that you specify, it returns HTTP status code 403 (Forbidden). For a full view of the request and response information, you can paste the **Request** command directly into the console and add the --debug argument.

***The scanner script uses an open source HTTP client called httpie (https://httpie.org/). HTTPie—aitch-tee-tee-pie—is a command line HTTP client with an intuitive UI, JSON support, syntax highlighting, wget-like downloads, plugins, and more.***

### Website Scanning Environment and Tools - Automated Scanning

In addition to the manual scanning, automated scanning is also performed against your lab website. The automated tests are similar to the manual tests but the results are posted to a [centralized scanning results dashboard](https://waflabdash-ap-southeast-2.awssecworkshops.com/) along with the other workshop participants. You can identify the scanning results for your site using the UniqueId found in the CloudFormation outputs.

![WAF Lab Centralized Dashboard](/images/waflabdash-pre.png)

You can now proceed to the [Remediate Phase](../perimeter-remediate).


---

### *Stuck? Watch this*

{{% notice note %}} 
*This video has no audio*{{% /notice %}}

{{< rawhtml >}}
<video width="696" height="392" controls>
  <source src="https://d1tqhetmq9f85b.cloudfront.net/downloads/lab3.2.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
{{< /rawhtml >}}