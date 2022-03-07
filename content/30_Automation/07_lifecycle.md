---
title: "Deploy Lifecycle"
chapter: false
weight: 34
---

### Automate Storage Stack deployment to new S3 buckets.

In this integration, we will deploy a CloudFormation template to ensure File Storage Security monitors any new S3 buckets that are created. Additionally anytime a S3 bucket resource is terminated, this template will automatically remove all deployed security resources to monitor the bucket.

![Diagram](/images/architecture.png)

---

#### Prerequisites  

**1.** Obtain your Cloud One Account Region.

- Sign into Cloud One
- Select **Account Settings**
- Copy down the Region: e.g us-1

![account](/images/account_setting.png)
![account](/images/region.png)

---

**2.** Under **Account Settings** in Cloud One, create a new API Key.

- Select **API Keys** from left-hand menu
- Click **New**

- **API Key Alias**: <code>immersion_day</code>
- **Description**: Optional
- **Role**: <code>Full Access</code>
- **Language**: preferred language
- **Language**: preferred language
- **Timezone**: preferred timezone
- Click **Next**
- Copy down your API Key in a safe place.

![account](/images/new_api.png)
![account](/images/define_api.png)


---

**3.** Obtain the name of the Scanner Stack and the Scanner Stacks SQS URL.

- Navigate to **AWS CloudFormation**
- Locate and select your deployed **Scanner Stack**
- Click the tab named **Outputs**.
- Copy down your **Scanner Stacks name**
- Locate the key **ScannerQueueURL**
- Copy down the **ScannerQueueURL** value.

![Diagram](/images/outputs.png)

---

### Deploy the CloudFormation template below

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=c1fss-lifecycle-workshop&templateURL=https://aws-workshop-c1as-cft-templates.s3.amazonaws.com/fss-lifecycle.yml)



---

**1.** Fill in the required template parameters with the values copied down previously. 

- **C1API**: paste your cloud one api key
- **C1RegionEndpoint**: paste your cloud one account region
- **SQSURL**: paste the sanner stack's sqs url value
- **StackName**: paste the name of your deployed Scanner stack
- Click **Next**
- Optional - Configure tags if desired.
- Click **Next**
- **Check the box** at the bottom to acknowledge IAM resource creation.
- Click **Create Stack**

![cft](/images/parameters.png)
![cft](/images/create_lifecycle.png)

---

**2.** Monitor the stack deployment until it reaches status: **Create_Complete**

![cft](/images/in_progress.png)
![cft](/images/complete.png)


---

### Test the automation

 Create a new S3 bucket

<details>
  <summary> -> <code>CLICK HERE</code> - <strong>Step by step instruction to create a S3</strong></summary>

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

</details>
----

![Diagram](/images/new_bucket.png)

---

#### After the S3 bucket has been created monitor CloudFormation to see the new Storage stack being deployed automatically.

- Wait for the stack to reach **create complete**

![Diagram](/images/automate1.png)
![Diagram](/images/automate2.png)

- Once the stack to reaches **create complete** check your Cloud One File Storage Console for the newly monitored bucket.

![Diagram](/images/automate3.png)

---

### Now delete the S3 bucket resource your just created

- In AWS navigate to **S3**
- Locate and select the bucket created in the last step
- Click **Delete**
- Confirm deletion by pasting bucket name and clicking **Delete bucket**

![Diagram](/images/terminate1.png)
![Diagram](/images/terminate2.png)

- Check **cloudformation** to see the stack being removed.

![Diagram](/images/terminate3.png)

- Once the stack is deleted check your Cloud One File Storage Console for the newly monitored bucket.

![Diagram](/images/terminate4.png)


----

#### Congratulations, you have successfully automated FSS to ensure new buckets are securely monitored!! :rocket::clap: