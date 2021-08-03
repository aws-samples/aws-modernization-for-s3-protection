---
title: "Quarantine Malicious Files"
chapter: false
weight: 33
---

### Prerequisites

1. **Create Amazon S3 buckets**
    - Create a 'Promote bucket' to receive clean files. Example: `fss-promote`.
    - Create a 'Quarantine bucket' to receive quarantined files. Example: `fss-quarantine`.
2. **Find the 'ScanResultTopic' SNS topic ARN**
    - In the AWS console, go to **Services > CloudFormation** > your all-in-one stack > **Resources** > your storage stack > **Resources**.
    - Scroll down to locate the  **ScanResultTopic** Logical ID.
    - Copy the **ScanResultTopic** ARN to a temporary location. It will look like this: `arn:aws:sns:us-east-1:123445678901:FileStorageSecurity-All-In-One-Stack-StorageStack-1IDPU1PZ2W5RN-ScanResultTopic-N8DD2JH1GRKF`

### Installation

In this case, let's use the [Serverless Application Repository](https://console.aws.amazon.com/serverlessrepo/home):

1. Visit [the app's page on the AWS Lambda Console](https://us-east-2.console.aws.amazon.com/lambda/home?region=us-east-2#/create/app?applicationId=arn:aws:serverlessrepo:us-east-1:415485722356:applications/cloudone-filestorage-plugin-action-promote-or-quarantine).
2. Fill in the parameters:
    * ScanResultTopic
    * ScanningBucketName
    * PromoteBucketName
    * QuarantineBucketName
    * Optionally, you can customize the name of the Cloud Formation stack that will be created
3. Check the `I acknowledge that this app creates custom IAM roles.` checkbox.
4. Click `Deploy`.

### Other Options

Check out [other installation methods](./other-installation-methods.md), such as AWS CLI or SAM CLI.

### Test the Application

To test that the application was deployed properly, you'll need to generate a malware detection using the [eicar test file](https://secure.eicar.org/eicar.com "A file used for testing anti-malware scanners."), and then check the Quarantine bucket to make sure the `eicar` file was sent there successfully.

1. **Download the Eicar test file**
   - Temporarily disable your virus scanner or create an exception, otherwise it will catch the `eicar` file and delete it.
   - Browser: Go to the [eicar file](https://secure.eicar.org/eicar.com) page and download `eicar_com.zip` or any of the other versions of this file.
   - CLI: `curl -O https://secure.eicar.org/eicar_com.zip`
2. **Upload the eicar file to the ScanningBucket**

    - Using the AWS console

        1. Go to **CloudFormation > Stacks** > your all-in-one stack > your nested storage stack.
        2. In the main pane, click the **Outputs** tab and then copy the **ScanningBucket** string. Search the string in Amazon S3 console to find your ScanningBucket.
        3. Click **Upload** and upload `eicar_com.zip`. File Storage Security scans the file and detects malware.
        4. Still in Amazon S3, go to your Quarantine bucket and make sure that `eicar.zip` file is present.
        5. Go back to your ScanningBucket and make sure the `eicar.zip` is no longer there.

        > 📌 It can take 15-30 seconds or more for the 'move' operation to complete, and during this time, you may see the file in both buckets.

    - Using the AWS CLI

        - Enter the folowing AWS CLI command to upload the Eicar test file to the scanning bucket:
            `aws s3 cp eicar_com.zip s3://<YOUR_SCANNING_BUCKET>`
        - where:
            - `<YOUR_SCANNING_BUCKET>` is replaced with the ScanningBucket name.

        > **NOTE:** It can take about 15-30 seconds or more for the file to move.

        You did it! :tada: 
        
        Using the AWS CLI or the AWS Console, you should be able to see the eicar file in your ```QuarantineBucketName``` with the correct tags.
