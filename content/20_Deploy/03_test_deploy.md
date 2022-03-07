---
title: "Testing your Deployment"
chapter: false
weight: 21
---

### Generating your First Detection

To test your deployment, you'll need to generate a malware detection using the eicar file. Follow the instructions below. 

This video here also demostrate the steps-by-steps if you prefer to check additional details before following the steps. 

{{< youtube id="2WDpQ7KgjRo" title="File Storage Security demo" >}}

---

**1.** In the AWS console, open **AWS CloudShell** in a new tab.

- To download the eicar test file, in CloudShell run this command: <code>wget https://secure.eicar.org/eicar.com.txt</code> 

![cloudshell](/images/wget.png)

- Upload the eicar file to the S3 bucket created previosuly: <code>aws s3 cp eicar.com.txt s3://**name-of-bucket-goes-here**/eicar.txt</code>

![cloudshell](/images/eicar.png)


---

**2.** After successful upload navigate to **S3** and find your S3 bucket that you defined to be scanned by File Storage Security.

![Diagram](/images/s3_1.png)

---

**2.4** Select the uploaded eicar file.

![cloudshell](/images/eicar-upload.png)

---

**3.** Examine the tags applied from the scan results. 

- Scroll to the <b>Tags</b> section, you will see the details like the example below:


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



