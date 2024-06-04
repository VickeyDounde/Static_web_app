# Hosting a Static Website in AWS

This project involves hosting a static website on AWS using S3, Route 53, Lambda, and DynamoDB. The website includes HTML, CSS, and JavaScript files, and captures user registration data, storing it in DynamoDB via a Lambda function.

## Step-by-Step Guide

### Step 1: Upload Files to S3

1. **Create an S3 Bucket**:
    - Go to the S3 console and create a new bucket.
    - Enable static website hosting from the bucket properties.
    
2. **Upload Website Files**:
    - Upload `index.html`, `styles.css`, and `script.js` to the S3 bucket.
    - Ensure the files are publicly accessible by setting the appropriate permissions.

### Step 2: Configure S3 Bucket Policy

Add a bucket policy to allow public read access to the objects in your bucket.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

### Step 3: Set Up Route 53 for Custom Domain

1. **Create a Hosted Zone**:
    - Go to Route 53 and create a hosted zone for your domain.

2. **Update DNS Nameservers**:
    - Update your domain's nameservers to point to the Route 53 nameservers.

3. **Create an Alias Record**:
    - Create an alias record in Route 53 to point your domain to the S3 bucket endpoint.

### Step 4: Create Lambda Function

Create a Lambda function to handle form submissions and store data in DynamoDB.

**Lambda Function Code**:
 
```python
import json
import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Static_web_app')

def lambda_handler(event, context):
    # Parse request body
    body = json.loads(event['body'])
    
    # Generate a unique ID for the item
    item_id = str(uuid.uuid4())

    # Create new item in DynamoDB table
    response = table.put_item(
        Item={
            'id': item_id,
            'email': body['email'],
            'name': body['name'],
            'phone': body['phone'],
            'password': body['password']
        }
    )

    # Return response
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps({'message': 'Registration successful'})
    }
```

### Step 5: Create DynamoDB Table

1. **Create Table**:
    - Go to the DynamoDB console and create a table named `Static_web_app`.
    - Set `id` as the primary key.

### Step 6: Set Up IAM Role

1. **Create IAM Role**:
    - Create an IAM role with full access to DynamoDB and CloudWatch.
    - Attach the role to the Lambda function.



