---
title: "Slack Integration"
chapter: false
weight: 33
---

{{% notice note %}}
<p style='text-align: left;'>
This is an <b>optional lab</b> if you would like to integrate notifications from Cloud One - File Storage Security to your Slack workspaces based on one specific channel that you will define.
</p>
{{% /notice %}}


In this integration, we will make a Lambda function send a Slack message every time that we have a new detection on Cloud One - File Storage Security, you can deploy this automation using the AWS Console, AWS CLI or Makefile. In this example we will demonstrate using the AWS CLI:


### Requirements

#### 1. Configure Slack Webhook App

- Create a Slack Channel to receive the notification
- Go to App Directory > Search `Incoming WebHooks`.
- Click on `Incoming WebHooks`, then click "Add to Slack"
- Choose the Channel to receive the notification
- Copy Webhook URL
- Enter the Description of your WebHook.
- Enter the Name of the Slack WebHook, by default it will use `incoming-webhook`; if you prefer, you can customize the name.
- If you want any custom icon to add that in Customize Icon section.
- Click "Save Setting"

![Diagram](/images/slack.png)

<b>If you need more detailf on how to create the Incoming Webhooks on Slack here is a great Link - [Additional information](https://slack.com/help/articles/115005265063-Incoming-webhooks-for-Slack)
</b>

---

#### 2. Find the ScanResultsTopic ARN.

In the AWS console, go to **Services > CloudFormation** > select the storage stack from File Storage Security > Click on **Resources**.
    - Scroll down to locate the  **ScanResultTopic** Logical ID. 
    - Copy the **ScanResultTopic** ARN to a temporary location. 
    
Example: ```arn:aws:sns:us-east-1:000000000000:FileStorageSecurity-All-In-One-Stack-StorageStack-1IDPU1PZ2W5RN-ScanResultTopic-N8DD2JH1GRKF```

![Diagram](/images/slack_2.png)

---

#### 2. Deploy the Slack plugin for File Storage Security:

##### 2.1 AWS CLI command to create the role:

- Open the Slack Serverless Application in a new tab: [CloudOne-FSS-Plugin-Slack](https://serverlessrepo.aws.amazon.com/applications/us-east-1/024368254241/CloudOne-FSS-Plugin-Slack) .

- Click **Deploy**

![Diagram](/images/sam.png)
![Diagram](/images/sam2.png)

----

### Fill in the parameters

- **ScanResultTopicARN**: the storage stack from File Storage Security ScanResultTopicARN
- **SlackChannel**: the name of your Slack channel created to receive notifications.
- **SlackURL**: the name of your incomming webhook Slack URL.
- **SlackUsername**: the Slack username to receive the notification on slack channel.

![Diagram](/images/sam3.png)
![Diagram](/images/sam4.png)


---

### Ensure the application reaches create complete

![Diagram](/images/sam5.png)

---

#### Now you can generate a new malware event as we did before in [Test Deploy](/20_deploy/03_test_deploy.html#generating-your-first-detection)

Then you should be able to see a Cloud One - File Storage Security event in your Slack Channel like this one below:

![Diagram](/images/slack_4.png)

---

{{% notice note %}}
<p style='text-align: left;'>
If you need more details on how to deploy the post action for <b>Slack integration</b> here is more information in our <a href="https://github.com/trendmicro/cloudone-filestorage-plugins/tree/master/post-scan-actions/aws-python-slack-notification">GitHub</a> repository. 
</p>
{{% /notice %}}