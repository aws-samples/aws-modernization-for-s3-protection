---
title: "Deploy Cloud One File Storage Security"
chapter: true
weight: 20
pre: "<b>3. </b>"
---


# Deployment


### S3 Bucket Creation
<details>
  <summary> -> <code>CLICK HERE</code> - <strong>If you don't have a S3 Bucket yet and you need to create one for this lab!</strong></summary>

**1.** Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/.
![Diagram](/images/create_s3.png)

---

**2.** Choose Create bucket. The Create bucket wizard opens.
![Diagram](/images/create_s3_2.png)


**3.** In Bucket name, enter a DNS-compliant name for your bucket.
The bucket name must:

- Be unique across all of Amazon S3.
- Be between 3 and 63 characters long.
- Not contain uppercase characters.
- Start with a lowercase letter or number.

After you create the bucket, you can't change its name. For information about naming buckets, see Bucket naming rules.

{{% notice warning %}}
<p style='text-align: left;'>
Remember that S3 bucket are an unique name globally for all AWS customers. If you try to use the same name from this workshop you will have some issues with an existing S3 bucket name already created.
</p>
{{% /notice %}}

![Diagram](/images/create_s3_3.png)

**4.** Scroll down and click on Create bucket. 

![Diagram](/images/create_s3_4.png)

---
**5.** Now you have successfully create a S3 bucket for the workshop.

![Diagram](/images/create_s3_5.png)

Let's start the Cloud One - File Storage Security deployment now. :laptop::cloud::rocket:
</details>
----

---

### Cloud One - File Storage Security Deployment

The best way to learn is to get our hands dirty, so let's deploy Cloud One - File Storage Security to our AWS Account. First of all, make sure that all the requirements are meet, then:

**1.** Login to [Cloud One platform](https://cloudone.trendmicro.com), it will request the login information for the Cloud One, in case you don't have it, check the requirements page that has all the details that you will need to create your account.  

![Diagram](/images/login.png)

---

**2.** Select the File Storage Security tile
![Diagram](/images/login_2.png)

---

**3.** Click on Stack management in your left side
![Diagram](/images/login_3.png)

---

**4.** Click on "Deploy" 
![Diagram](/images/login_4.png)

---

**5.** Select ```Scanner Stack and Storage Stack```, to deploy the full solution to your AWS account.

![Diagram](/images/fss-deploy-stacks-select.png)

---

**6.** Select the AWS region that best suit for you ([AWS regions supported](/10_requirements.html)) and click on ```Launch Stack```, this will redirect you to your AWS account in the region that you choose to deploy the stack. Make sure that you're logged in and has the correct permissions, you can check the details of the permissions required in the Requirements section.

{{% notice tip %}}
Try to make sure the Cloud One console tab is in the same Window from your browser this way the Launch Stack will automatically use the AWS session that you have open already. 
{{% /notice %}}

You can validate the Cloud Formation Template by clicking in ```Review Stack```, to make it easier, you can also verify the Cloud Formation Template by clicking the buttons below:

{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-All-In-One.template" %}}All in One Template{{% /button %}}
{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-Scanner-Stack.template" %}}Scanner Stack{{% /button %}}
{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-Storage-Stack.template" %}}Storage Stack{{% /button %}}

![Diagram](/images/login_5.png)

---

**7.** In the CloudFormation page the <b>only required parameter</b> here is the <b>name of bucket</b> that you choose to be scanned, just add it in the configuration **(S3BucketToScan)**. In our case please use the name from the S3 bucket that you have created early in this workshop or use one existing S3 bucket that you prefer.

It also supports differents parameters to customize your installation, like Resource prefixes and optional KMS integration, for more details about these configurations check our <a href="https://cloudone.trendmicro.com/docs/file-storage-security/gs-deploy-all-in-one-stack/">Documentation</a>.

After adding the bucket name you will need to acknowledge and click on <b>"Create Stack"</b>

![Diagram](/images/cfdeploy.png)

---

**8.** After deploying the all-in-one stack in your AWS account, you must configure the scanner and storage stack Amazon Resource Names (ARNs) information in Cloud One console. The ARNs map a scanner stack to a storage stack, allowing them to be aware of each other.

Go to CloudFormation > Stacks > your all-in-one stack > Outputs tab and copy the Value with these Key names here ```ScannerStackManagementRoleARN``` and ```StorageStackManagementRoleARN``` and paste the information into Cloud One - File Storage Security console.

![Diagram](/images/fss-arn-aws-info.png)

![Diagram](/images/login_6.png)

---

**9.** Then Click <b>Submit</b>. You see a couple of success messages at the bottom and the stack will show in the Stack Management too. 

![Diagram](/images/login_7.png)

You have now completed the deployment of the All-in-One stack :tada:, so let's test our deployment :laptop:

All the new objects that will be uploaded to your bucket that you define to be scan, now will be automatically scanning against malware and tagged as "malicious" or "no issues found".


---

Here is a video with the steps-by-steps on how to deploy All-in-One stack for File Storage Security:
{{< youtube id="_w1_k5W3tEQ" title="Deploying the File Storage Security All In One Stack" >}}


