Overview
The ability to start and stop Amazon EC2 instances at predefined intervals can be useful for optimizing resource utilization and cost control in an AWS environment. AWS Lambda, a serverless computing tool, may do this automation without the server or infrastructure management requirement. This review will walk you through the processes required in configuring a Lambda function to start or stop Amazon EC2 instances on a preset schedule


step 1: Create an Amazon EC2 Instance:

Search Amazon EC2 in the service tab
Click on the launch Instance
Choose the name of the instance, size of the instance, OS, VPC, Security group, EBS, and other resources as per requirements.
![image](https://github.com/user-attachments/assets/5934af31-4653-4fef-9aeb-41b064bb7bb5)

step 2: Create AWS IAM Policy using the JSON policy editor
![image](https://github.com/user-attachments/assets/b7c642c9-3a4d-4b76-9098-b3eac1f3b5ee)

![image](https://github.com/user-attachments/assets/c0c93dee-c384-4cc1-93c0-77d94f79d4a6)
![image](https://github.com/user-attachments/assets/5051fb89-4bcb-40dc-867d-9a9649387f9f)


Step 3: Create AWS IAM Role

Choose AWS IAM in the service tab
Click on Create Role
Choose the AWS Service option and AWS Lambda as a use case
Attach the policy which you create in Step 2
Choose to Create Role


![image](https://github.com/user-attachments/assets/c398c968-0f58-4752-97b3-ac78aa122dda)

Step 4: Create AWS Lambda functions that stop and start your Amazon EC2 instances

Choose to Create function
Choose an Author from scratch
For the Function name, please enter a name that identifies it as the function used to stop your Amazon EC2 instances.
For Runtime, choose Python 3.9
Under Permissions, expand Change default execution role
Under the Execution role, choose to Use an existing role
Under Existing role, choose the IAM role that you created
Choose Create function
Under Code, Code source, copy and paste the following code into the editor pane.
Choose Deploy
On the Configuration tab, choose General Configuration, Edit. Set Timeout to 10 seconds, and then choose

import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances)




Step 5: Repeat all the steps in step 4 to Create AWS Lambda Function to start your Amazon EC2 Instance

Under Code, Code source, copy and paste the following code into the editor pane
Start the instances:-

import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))


Step 6: Test Your Lambda Function

Open the AWS Lambda Console, and then choose Functions.
Choose one of the functions that you created
Choose the Code In the Code source section, choose Test
In the Configure test event dialog box, choose to Create a new test event
Enter an Event name. Then, choose Create
Choose Test to run the function
Repeat steps 1-7 for the other function that you created.


![image](https://github.com/user-attachments/assets/0aeec74f-793e-4bcb-8bf9-b2bc5ac34bb1)

 Check the status of your Amazon EC2 instances



Step 8: Create Amazon EventBridge rules to run AWS Lambda functions

Open the Amazon EventBridge Console
Select Create rule
Enter a Name for your rule
For Rule type, choose Schedule, and then choose to Continue in Amazon EventBridge Scheduler
For Schedule pattern, choose Recurring schedule
When Schedule type is Cron-based schedule, for the Cron expression, enter an expression that tells Lambda when to stop your instance, and Cron expressions are evaluated in UTC
In Select targets, choose AWS Lambda function from the Target
For Function, choose the function that stops your Amazon EC2 instances
Repeat steps 1-8 to create a rule to start your Amazon EC2 instances


![image](https://github.com/user-attachments/assets/dc09e10a-57cb-40de-9693-905a080ab59b)
![image](https://github.com/user-attachments/assets/37756f02-d275-46a9-a503-995f889d015d)
![image](https://github.com/user-attachments/assets/6a735b63-b2a7-46b5-bd8c-60741598a5dd)
![image](https://github.com/user-attachments/assets/a73a7584-f9e1-4150-ab77-bd3efdebe307)
![image](https://github.com/user-attachments/assets/f169d769-a19c-4581-bb0a-56879a58f06b)




Conclusion
AWS Lambda to start and stop Amazon EC2 instances can provide many benefits, including cost savings, automation of routine tasks, and improved security. However, it’s important to configure AWS IAM roles and policies to ensure that only authorized users can access and perform actions on Amazon EC2 instances. There are also some limitations to using AWS Lambda, such as the maximum execution time of 15 minutes per function and the potential for cold start times. Using Lambda to manage Amazon EC2 instances can be useful for optimizing and automating your AWS environment.











    
