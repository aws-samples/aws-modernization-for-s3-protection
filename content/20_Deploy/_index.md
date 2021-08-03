---
title: "Deploy Cloud One File Storage Security"
chapter: true
weight: 20
pre: "<b>3. </b>"
---


# Deployment

The best way to learn is to get our hands dirty, so let's deploy Cloud One - File Storage Security to our AWS Account. First of all, make sure that all the requirements are meet, then:

Go to Cloud One - File Storage Security, here: https://cloudone.trendmicro.com/filestorage/deployment, it will request the login information for the Cloud One, in case you don't have it, check the requirements page that has all the details that you will need to create your account. 

![Diagram](/images/fss-deploy-stacks.png)

Click on "Deploy" and select ```Scanner Stack and Storage Stack```, to deploy the full solution to your AWS account.

![Diagram](/images/fss-deploy-stacks-select.png)

Select the AWS region that best suit for you and click on ```Launch Stack```, this will redirect you to your AWS account in the region that you choose to deploy the stack. Make sure that you're logged in and has the correct permissions, you can check the details of the permissions required in the Requirements section.

You can validate the Cloud Formation Template by clicking in ```Review Stack```, to make it easier, you can also verify the Cloud Formation Template by clicking the buttons below:

{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/templates/FSS-All-In-One.template" %}}All in One Template{{% /button %}}
{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/templates/FSS-Scanner-Stack.template" %}}Scanner Stack{{% /button %}}
{{% button href="https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/templates/FSS-Storage-Stack.template" %}}Storage Stack{{% /button %}}
![Diagram](/images/fss-launch-stacks.png)

In the Cloud Formation page the <b>only required parameter</b> here is the <b>name of bucket</b> that you chose to be scanned, which is represented by the ``` S3BucketToScan ```. It also supports differents parameters to customize your installation, like Resource prefixes and optional KMS integration, for more details about these configurations check our <a href="https://cloudone.trendmicro.com/docs/file-storage-security/gs-deploy-all-in-one-stack/">Documentation</a>.
After adding the bucket name you will need to acknowledge and click on <b>"Create Stack"</b>

![Diagram](/images/cfdeploy.png)

After deploying the all-in-one stack in your AWS account, you must configure the scanner and storage stack Amazon Resource Names (ARNs) information in Cloud One console. The ARNs map a scanner stack to a storage stack, allowing them to be aware of each other.

Go to CloudFormation > Stacks > your all-in-one stack > Outputs tab and copy the content with these names here ```ScannerStackManagementRoleARN``` and ```StorageStackManagementRoleARN``` and paste the information into Cloud One - File Storage Security console.

![Diagram](/images/fss-arn-aws-info.png)

![Diagram](/images/fss-arn.png)

Then Click <b>Submit</b>. You see a couple of success messages at the bottom.

![Diagram](/images/fss-two-stacks.png)

You have now completed the deployment of the All-in-One stack :tada:, so let's test our deployment :laptop: