# Auto-Tagging-EC2-Instances-on-Launch-Using-AWS-Lambda-and-Boto3
Whenever you launch a new EC2 instance, AWS CloudWatch detects the event and triggers your Lambda function, which tags the instance automatically—no manual action needed.

# Step-by-Step Process

1.CloudWatch Event Rule Setup

You create a CloudWatch Event Rule that listens for EC2 instance launch events (specifically, when an instance enters the pending or running state).
Lambda Function Triggered

When a new EC2 instance is launched, the CloudWatch Event Rule automatically triggers your Lambda function.
Lambda Receives Event Data

The Lambda function receives an event payload containing details about the launched EC2 instance, including its instance ID.
Lambda Uses Boto3 to Tag Instance

2.The Lambda function:
Extracts the instance ID from the event.
Gets the current date.
Creates a list of tags (e.g., LaunchDate and a custom tag like Owner).
Calls the Boto3 create_tags method to apply these tags to the new instance.
Confirmation Logging.

Code: import boto3
from datetime import datetime

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Extract instance ID from the event
    instance_id = event['detail']['instance-id']
    
    # Prepare tags
    current_date = datetime.utcnow().strftime('%Y-%m-%d')
    tags = [
        {'Key': 'LaunchDate', 'Value': current_date},
        {'Key': 'Owner', 'Value': 'AutoTagged'}  # Custom tag
    ]
    
    # Tag the instance
    ec2.create_tags(Resources=[instance_id], Tags=tags)
    print(f"Tagged instance {instance_id} with {tags}")


The Lambda function prints a confirmation message for logging and troubleshooting.
Result

The new EC2 instance is automatically tagged with the current date and your custom tag, visible in the AWS EC2 console.




