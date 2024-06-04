# Static_web_app
just a simple registration form

# Creating a markdown summary for hosting a static website in AWS with S3, Route 53, Lambda, and DynamoDB

summary_content_static_website = """
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

