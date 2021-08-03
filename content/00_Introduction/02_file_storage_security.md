---
title: "Cloud One -  File Storage Security"
chapter: false
weight: 7
---

### File Storage Security

File Storage Security is part of the Trend Micro Cloud One™ security service platform, helping your organization to build and run applications securely by offering controls that work across your existing infrastructure or modern code streams, development toolchains, and multiplatform requirements. File Storage Security helps ensure your Amazon S3 buckets are free from malware by deploying cloud-native security that can be integrated into your custom Amazon S3 workflows.

File Storage Security is backed by Trend Micro Research, which continuously monitors and collects threat data from across the globe by employing advanced detection analytics to immediately block attacks before they can harm your organization.

### Understanding How Trend Micro Can Help Secure Your Object Storage

There are two main ways to use File Storage Security in your AWS Infrastructure:

- <b>File Upload Scan</b> -> Any cloud-native application that uses the power of the cloud to provide features integrated with Amazon S3 buckets could be exposed to malware. File Storage Security allows customers to integrate scan capabilities directly to their AWS accounts, so all of the files you receive in your Amazon S3 buckets from external sources can be scanned for malware before your application can consume the data.

- <b>Automated Workflow</b> -> Development teams leverage event-driven designs with Amazon S3 to automate the processing of data uploaded to a bucket. File Storage Security was designed with this architecture in mind allowing development teams to seamlessly integrate file scanning into their automated workflow.

### Architecture


Cloud-native application architectures incorporate cloud file/object storage services into their workflow, creating a new attack vector where they are vulnerable to malicious files. File Storage Security protects the workflow through innovative techniques, such as malware scanning, integration into your custom workflows, and broad cloud storage platform support – freeing you to go further and do more. File Storage Security provides flexible implementation options, including a
multi-bucket promotional model (scanning bucket to quarantine/clean bucket)
or an efficient single-bucket architecture. 


![Diagram](/images/fss.png)


File Storage Security’s architecture was built to be simple to understand and monitor all of the buckets. As soon as a new file is uploaded to the bucket, this will generate a SQS message inside your AWS account that will trigger an AWS Lambda function. The function will execute the scan and tag the file as malicious or clean, depending on the scan result. It is also possible to connect plugins to perform additional actions, for example, as soon the file is tagged as malicious, the plugin moves the tagged file to a quarantine bucket.

Digging a little deeper into the architecture details, the overall deployment is made up of two different stacks:

- <b>Storage Stack</b>: This stack is responsible for accepting the notification for the Amazon S3 bucket, as well as sending newly uploaded files to the Scanner Stack for the security scan. After the scan is complete, an Amazon SNS topic is published and the file is tagged as “malicious” or “clean”—additional plugins are available to add more functionality to the stack.

- <b>Scanner Stack</b>: This stack is responsible for executing the scan and publishing the results to the Amazon SNS ScanResultTopic. When the Scanner Stack receives the request from the Storage Stack, its processes it and uses an AWS Lambda function to execute the scan. Like many Trend Micro technologies, File Storage Security can leverage Trend Micro™ Smart Protection Network™ for the latest threat information.

![Diagram](/images/fss_architecture.png)

{{% notice info %}}
<p style='text-align: left;'>
In order to launch both stacks, you must leverage the following CloudFormation Templates: <b>FSSScanner-Stack.template</b> and <b>FSS-Storage-Stack.template</b>. We also offer an all-in-one stack that launches both, called <b>FSS-All-In-One.template</b>. To help security and DevOps teams automate their workflows with File Storage Security, all CloudFormation Templates are available in our <a href="https://github.com/trendmicro/cloudone-filestorage-cloudformation-templates/tree/master/templates">GitHub</a> repository. For additional plugins, we have created a separate <a href="https://github.com/trendmicro/cloudone-filestorage-plugins/tree/master/post-scan-actions">GitHub</a> repository to enable you to add more functionality, including post-scan actions to permit files to either be pass or be quarantined.
</p>
{{% /notice %}}