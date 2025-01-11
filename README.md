Create an IAM policy and role for your Lambda function
Complete the following steps:

Use the JSON policy editor to create an IAM policy. Enter the following JSON policy document into the policy editor:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Start*",
                "ec2:Stop*"
            ],
            "Resource": "*"
        }
    ]
}
Create an IAM role for Lambda.

Attach the IAM policy to the IAM role.

Note: If you use an Amazon Elastic Block Store (Amazon EBS) volume, then additional configuration might be required. If the Amazon EBS volume is encrypted with a customer-managed AWS Key Management Service (AWS KMS) key, then add kms:CreateGrant to the IAM policy.

Create Lambda functions that stop and start your instances
Complete the following steps:

Open the Lambda console, and then choose Create function.

Choose Author from scratch.

Under Basic information, enter the following information:
For Function name, enter a name that describes the function, such as StopEC2Instances or StartEC2Instances.
For Runtime, choose Python 3.9.
Under Permissions, expand Change default execution role.
Under Execution role, choose Use an existing role.
Under Existing role, choose the IAM role.

Choose Create function.

Choose the Code tab.

On the lambda_funciton.py tab under Code source, enter the following code into the code editor to stop_instances:

import boto3region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
To start_instances, enter the following code into the code editor:

import boto3region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))
Note: Replace us-west-1 with the AWS Region that your instances are in and InstanceIds with the IDs of the instances that you want to stop and start.

Choose Deploy.

On the Configuration tab, choose General configuration, and then choose Edit.

Set Timeout to 10 seconds, and then choose Save.

Test your Lambda functions
Complete the following steps:

Open the Lambda console, and then choose Functions.
Choose one of the functions.
Choose the Code tab.
In the Code source section, choose Test.
In the Configure test event dialog box, choose Create new test event.
Enter an event name, and then choose Create.
Note: Don't change the JSON code for the test event.
Choose Test to run the function.
Repeat steps 1-7 for the other function.
Check the status of your instances
Amazon EC2 console

Before and after you test, check the status of your instances to confirm that your functions work.

CloudTrail

You can also use AWS CloudTrail to confirm that the Lambda function stopped or started the instance.

Complete the following steps:

Open the CloudTrail console.
In the navigation pane, choose Event history.
On the Lookup attributes dropdown list, choose Event name.
In the search bar, enter StopInstances to review the results. Then, enter StartInstances.
If there are no results, then the Lambda function didn't stop or start the instances.

Create EventBridge rules that run your Lambda functions
Complete the following steps:

Open the EventBridge console.
Choose Create rule.
Enter a name for your rule, such as StopEC2Instances or StartEC2Instances.
(Optional) For Description, enter a description for the rule.
For Rule type, choose Schedule, and then choose Continue in EventBridge Scheduler.
In the Schedule pattern, for Occurrence, choose Recurring schedule.
For Schedule type, choose either rate-based schedule or cron-based schedule, and then complete one of the following steps:
For Rate-based schedule, enter a rate value, and then choose an interval of time in minutes, hours, or days.
-or-
For Cron-based schedule, enter an expression that tells Lambda when to stop or start your instance.
Note: Cron expressions are evaluated in UTC. Make sure that you adjust the expression for your time zone.
On the Select targets page, choose Lambda function from the Target dropdown list.
For Function, choose the function that stops or starts your instances.
Choose Skip to review and create, and then choose Create.
