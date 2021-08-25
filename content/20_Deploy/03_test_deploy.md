---
title: "Testing your Deployment"
chapter: false
weight: 21
---

### Generating your First Detection

To test your deployment, you'll need to generate a malware detection using the eicar file. Follow the instructions below. 

This video here also demostrate the steps-by-steps if you prefer to check additional details before following the steps.

{{< youtube id="2WDpQ7KgjRo" title="File Storage Security demo" >}}


**1.** Obtain the EICAR (European Institute for Computer Antivirus Research) file, this is a test file developed for a simple test of anti-malware engine:

- Temporarily disable your virus scanner, otherwise it will catch the eicar file and delete it. Go to the <a href="https://www.eicar.org/">EICAR</a> file page and download ```eicar_com.zip``` or any of the other versions of this file.

**2.** Upload the eicar file to your Amazon S3 bucket that we define before to be scanning by Cloud One - File Storage Security.

**2.1** Go to AWS console

**2.2** After go to Services > S3 and find your S3 bucket that you defined to be scanned by File Storage Security

![Diagram](/images/s3_1.png)

**2.3** click <b>Upload</b> and upload the file that you downloaded. Cloud One - File Storage Security will scan the file, detects as a malware, and will add tags information about it in the object.
![Diagram](/images/s3_2.png)

Click Add files, select the eicar and click **Upload**.

![Diagram](/images/s3_3.png)

---

Or you could upload the file using the AWS CLI using the example:

``` bash
aws s3 sync eicar.txt s3://my-bucket/path
```
To know more on how to use the AWS CLI, check the <a href="https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html">Installation</a> and <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html">Configuration</a> documentation.

---

**3.** Now that you have the object uploaded to Amazon S3 you can examine the tags from the scan results. In your Amazon S3 bucket, click in the file that you uploaded, then scroll to the <b>Tags</b> section, you will see the details like the example below:


![Diagram](/images/tags_updated.png)

Look for the following tags:
``` bash
fss-scan-date "date_and_time"
fss-scan-result "malicious"
fss-scanned "true"
```
The tags indicate that File Storage Security scanned the file and tagged it correctly as malware. You have now tested your File Storage Security deployment :+1:

You could also check in the Cloud One - File Storage Security dashboard to look for how many files have been scanned and recognized as malicious. 

![Diagram](/images/s3_4.png)

{{% notice note %}}
<p style='text-align: left;'>
You could do the same process with a safe file and see how Cloud One - File Storage Security will be tagging it in the object.
</p>
{{% /notice %}}


{{% notice warning %}}
<p style='text-align: left;'>
Remember to re-enable your anti-malware solution in your laptop after the test is complete.
</p>
{{% /notice %}}


