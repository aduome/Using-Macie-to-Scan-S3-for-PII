# Using-Macie-to-Scan-S3-for-PII
This documentation explains how to use Amazon Macie to scan data in Amazon S3 and send notifications when personally identifiable information (PII) is detected.

**Disclaimer:** Amazon Macie is free for the first 30 days; ensure to disable Macie before the trial period ends to avoid charges.

## Introduction
Amazon Macie is a powerful service that uses machine learning and pattern matching techniques to discover, classify, and protect sensitive data, such as Personally Identifiable Information (PII). In this guide, we'll walk through the steps to configure Macie to scan Amazon S3 for PII and set up notifications using EventBridge and SNS.

The following AWS services will be used:

- Amazon S3: To store data
- Amazon Macie: To scan the S3 data and detect PII
- Amazon EventBridge: To trigger actions based on Macie events
- Amazon SNS: To send email notifications
## Prerequisites
- An AWS account
- Amazon S3 bucket created to store data
- Amazon Macie enabled in your account
- Amazon EventBridge rule created for Macie event
- Amazon SNS topic created for notifications
- AWS CLI installed and configured with appropriate credentials

## Example Data

We will use the following example PII data for this demonstration:

- Credit Card Information: `cc.txt`
[Get CC files here](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Example%20Files/cc.txt)
- Employee Information: `employees.txt`
[Get Employee Information here](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Example%20Files/employees.txt)
- Access Credentials: `keys.txt`
[Get keys here](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Example%20Files/keys.txt)
- Custom Data (Australian Licence Plates): `plates.txt`
[Get plates here](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Example%20Files/plates.txt)

## Task 1 - Add Example Data to S3

1. Save the provided example data as text files.
2. Navigate to the [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/buckets) (region: us-east 1).
3. Create a new S3 bucket.
4. Upload the example files to the S3 bucket.

![-](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Project%20Images/1.%20S3%20Uploaded%20files_Example%20files.png)

Optional: Use AWS CLI to upload files to random subdirectories.

```bash
# Example AWS CLI command
for x in $(ls -1 *.txt); do aws s3 cp ${x} s3://BUCKET_NAME/$RANDOM/$RANDOM/$RANDOM/${x}; done
```

## Task 2: Enable Amazon Macie
- Navigate to the [Macie console](https://us-east-1.console.aws.amazon.com/macie/home?region=us-east-1#home)

- Enable Macie and select the S3 bucket created earlier for scanning.

  ![-](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Project%20Images/2.%20Macie%20-%20Select%20Specific%20bucket.png)

- Create a Macie job for the selected bucket.
  
![-](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Project%20Images/3.%20Macie%20-%20Select%20one-time%20job.png)

## Task 3: Setting up SNS
- Navigate to the [Amazon SNS Console](https://us-east-1.console.aws.amazon.com/sns/v3/home?region=us-east-1#/homepage)

- Create an SNS topic named "Macie-Alerts".

[-](https://github.com/aduome/Using-Macie-to-Scan-S3-for-PII/blob/main/Project%20Images/4.%20SNS%20topic%20created.png)

- Subscribe your email to the topic for notifications.

  
  
- Confirm your subscribed email.

- Create SNS Topic

## Task 4: Setting up EventBridge
- Navigate to the [Amazon EventBridge Console](https://us-east-1.console.aws.amazon.com/events/home?region=us-east-1#)

- Create an EventBridge rule to trigger on Macie findings and send events to the SNS topic.

## Task 5: Adding a Custom Macie Data Identifier
- Navigate to the [Macie console](https://us-east-1.console.aws.amazon.com/macie/home?region=us-east-1#home)
- Create a custom data identifier for Australian license plates using the provided regular expression.

## Task 6: Starting a New Job
- Start a Macie job to scan the S3 bucket for PII using the custom data identifier.

## Task 7: Clean up
- Delete the resources created in this demonstration (SNS topic, EventBridge rule, Macie job, S3 bucket).
