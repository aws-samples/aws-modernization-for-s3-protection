---
title: "Slack Integration"
chapter: false
weight: 32
---


In this integration, we will make a Lambda function send a Slack message every time that we have a new detection on Cloud One - File Storage Security, you can deploy this automation using the AWS Console, AWS CLI or Makefile. In this example we will demonstrate using the AWS CLI:


### Requirements

1. **Install and Configure the AWS CLI**
        - Install the AWS command line interface (CLI). See [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) for details.
2. **Configure Slack Webhook App**
    - Create a Slack Channel to receive the notification
    - Go to App Directory > Search `Incoming WebHooks`.
    - Click on `Incoming WebHooks`, then click "Add to Slack"
    - Choose the Channel to receive the notification
    - Copy Webhook URL
    - Enter the Description of your WebHook.
    - Enter the Name of the Slack WebHook, by default it will use `incoming-webhook`; if you prefer, you can customize the name.
    - If you want any custom icon to add that in Customize Icon section.
    - Click "Save Setting"
    
    [Additional information](https://slack.com/help/articles/115005265063-Incoming-webhooks-for-Slack)

3. **Create an execution role for the Lambda function, enter the following AWS CLI command to create the role:**

```
LAMBDA_TRUST="{
        \"Version\": \"2012-10-17\",
        \"Statement\": [
            {
                \"Action\": \"sts:AssumeRole\",
                \"Effect\": \"Allow\",
                \"Principal\": {
                    \"Service\": \"lambda.amazonaws.com\"
                }
            }
        ]
    }"
```
```    
aws iam create-role --role-name <YOUR_FSS_LAMBDA_SLACK_NOTIFICATION_ROLE> --assume-role-policy-document "${LAMBDA_TRUST}"
```

Where the ```<YOUR_FSS_LAMBDA_SLACK_NOTIFICATION_ROLE>``` is replaced with the name you want to give to the role. Example: ***FSS_Lambda_Slack_Notification_Role***

2. Attach the `AWSLambdaBasicExecutionRole` managed policy to the role:

```
aws iam attach-role-policy --role-name FSS_Lambda_Slack_Notification_Role --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

### Deploy the Lambda

1. Download the Lambda function [handler.py file from GitHub](https://github.com/trendmicro/cloudone-filestorage-plugins/blob/master/post-scan-actions/aws-python-slack-notification/handler.py).
2. In a shell program, create a deployment package:

```
zip <YOUR_ZIP_NAME>.zip handler.py
```

Where ```<YOUR_ZIP_NAME>``` is replaced with the name you want to give your Lambda function. Example: **scan-send-slack-notification**

3. Create the Lambda function, using backslashes (`\`) to separate the lines, as shown below:

```
aws lambda create-function --function-name <YOUR_FSS_SCAN_SEND_SLACK_NOTIFICATION> \
--role <YOUR_FSS_LAMBDA_SLACK_NOTIFICATION_ROLE> \
--runtime python3.8 \
--timeout 30 \
--memory-size 512 \
--handler handler.lambda_handler \
--zip-file fileb://<YOUR_ZIP_NAME>.zip \
--region <YOUR_REGION> \
--environment Variables=\{SLACK_URL=<YOUR_SLACK_URL>,SLACK_CHANNEL=<YOUR_SLACK_CHANNEL>,SLACK_USERNAME=<YOUR_SLACK_USERNAME>\}
```

- where:
    - `<YOUR_FSS_SCAN_SEND_SLACK_NOTIFICATION>` is replaced with the name you want to give your Lambda function. Example: `FSS_Scan_Send_Slack_Notification`.
    - `<YOUR_FSS_LAMBDA_SLACK_NOTIFICATION_ROLE>` is replaced with the ARN of the role you previously created for the Lambda function. You can find the ARN in the AWS console under **Services > IAM > Roles** > your role > **Role ARN** field (at the top). Example: `arn:aws:iam::000000000000:role/FSS_Lambda_Slack_Notification_Role`.
    - `<YOUR_ZIP_NAME>` is replaced with the name of the ZIP file you created earlier. Example: `scan-send-slack-notification`
    - `<YOUR_REGION>` is replaced by the region where the scanning bucket resides
    - `<YOUR_SLACK_URL>` is replaced with the name of your incomming webhook Slack URL.
    - `<YOUR_SLACK_CHANNEL>` is replaced with the name of your Slack channel created to receive notifications.
    - `<YOUR_SLACK_USERNAME>`is replaced with the subject of your Slack username to receive the notification on slack channel

### Subscribe the Lambda to the SNS topic

1. **Find the 'ScanResultTopic' SNS topic ARN** 
    - In the AWS console, go to **Services > CloudFormation** > your all-in-one stack > **Resources** > your storage stack > **Resources**.
    - Scroll down to locate the  **ScanResultTopic** Logical ID. 
    - Copy the **ScanResultTopic** ARN to a temporary location. Example: ```arn:aws:sns:us-east-1:000000000000:FileStorageSecurity-All-In-One-Stack-StorageStack-1IDPU1PZ2W5RN-ScanResultTopic-N8DD2JH1GRKF```
2. **Find the Lambda function ARN**
    
    📌 The Lamdba function ARN is required only if you plan to use the AWS CLI (as opposed to the console) to subscribe the Lambda to the SNS topic.
    - In the AWS console, go to **Services > Lambda**.
    - Search for the Lambda function you created previously. Example: `FSS_Scan_Send_Slack_Notification`
    - Click the Lambda function link.
    - On the top-left, locate the **ARN**.
    - Copy the ARN to a temporary location. Example: `arn:aws:lambda:us-east-1:000000000000:function:FSS_Scan_Send_Slack_Notification`
    - Enter the following AWS CLI command to subscribe your Lamdba function to the SNS topic:
        
        `aws sns subscribe --topic-arn <SNS_TOPIC_ARN> --protocol lambda --notification-endpoint <YOUR_LAMBDA_FUNCTION_ARN> --region <YOUR_REGION>`
    - where:
        - `<SNS_TOPIC_ARN>` is replaced with the SNS topic ARN you found earlier.
        - `<YOUR_LAMBDA_FUNCTION_ARN>` is replaced with the Lambda function ARN you found earlier.
        - `<YOUR_REGION>` is replaced by the region where the scanning bucket resides
    - Lastly, grant the SNS service permission to invoke your function.

        `aws lambda add-permission --function-name <FUNCTION_NAME> --region <YOUR_REGION> --statement-id sns --action lambda:InvokeFunction --principal sns.amazonaws.com --source-arn <SNS_TOPIC_ARN>`
    - where:
        - `<FUNCTION_NAME>` is replaced by the name of the Lambda function you created previously. Example: `FSS_Scan_Send_Slack_Notification`
        - `<YOUR_REGION>` is replaced by the region where the scanning bucket resides
        - `<SNS_TOPIC_ARN>` is replaced with the SNS topic ARN you found earlier.


{{% notice note %}}
<p style='text-align: left;'>
If you need more details on how to deploy the post action for <b>Slack integration</b> here is more information in our <a href="https://github.com/trendmicro/cloudone-filestorage-plugins/tree/master/post-scan-actions/aws-python-slack-notification">GitHub</a> repository. 
</p>
{{% /notice %}}