<h1>Cloud Uploader CLI</h1>


## Introduction

Hi! Welcome to my project. This project comes from the [Learn to Cloud Guide](https://learntocloud.guide/phase1/#capstone-project-clouduploader-cli), a structured and carefully put together guide on building skills suited for cloud engineering. I really enjoyed the process of this project because not only did it deal with AWS but it was also through PowerShell, which I have been excited to work in. As I continue my career, I have been curious to learn new things and advance my career as an IT professional. Down below I’ll guide you through the project for you to follow along.



<h2>Languages and Utilities Used</h2>

- <b>PowerShell: Primary scripting language used to create the CLI tool.</b> 
- <b>AWS CLI: Used to interact with AWS S3 for file uploads.</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (22H2)

## Project Overview

Initially, this project was intended to be run through the bash CLI but I was more interested in creating it as a powershell script. The powershell script will upload a file(s) to a cloud storage from the CLI. Since I am familiar with AWS services and want to gain scripting experience, it seemed like a great idea to bridge both technologies to further enhance my learning.

### Step 1: Setting up AWS
Before jumping into building the script, start by setting up the AWS services needed for the script to function properly. Below you’ll find the services used:

|Service    |Description    |
|:----------|:----------|
|IAM|Identity and Access Management – helps securely control access to AWS resources by managing users, groups, and permissions|
|S3|Simple Storage Solution – scalable storage that lets you store and retrieve data from the web |

Create a user and user group with all permissions enabled to the specific user group, this can be done by attaching the <i> AmazonS3FullAccess</i> policy. The policy will enable the user group to interact and use the S3 bucket but <b> beware of using this policy in a professional environment.</b> On the user’s dashboard go into the *'Security Credentials'* tab and generate new access keys to be used when authenticating the AWS CLI. Once done with that navigate to the S3 service and create a bucket with all the default settings. 

### Step 2: Setup AWS CLI
Go ahead and start by making sure you have the AWS CLI installed and configured on your system that can be downloaded [here](https://aws.amazon.com/cli/). Once installed, open powershell and run `aws configure` this will prompt you to input your AWS Access Key, Secret Key, default region name, and output format.

### Step 3: Create the PowerShell Script
If you haven't already installed a text editor I recommend you do so, Visual Studio code is what I used. In the text editor create a new file named ***‘clouduploader.ps1’*** and save it, add the following contents to the file:

 ```
param (
    [string]$FilePath,
    [string]$BucketName = "your-s3-bucket-name" # Replace with your bucket name
)

function Show-Usage {
    Write-Host "Usage: .\clouduploader.ps1 -FilePath /path/to/file.txt [-BucketName your-s3-bucket-name]"
    exit 1
}

# Check if the file path is provided
if (-not $FilePath) {
    Show-Usage
}

# Check if the file exists
if (-not (Test-Path -Path $FilePath)) {
    Write-Host "File not found: $FilePath"
    exit 1
}

# Upload the file to S3
try {
    aws s3 cp $FilePath "s3://$BucketName/$(Split-Path -Leaf $FilePath)"
    if ($?) {
        Write-Host "File successfully uploaded to s3://$BucketName/$(Split-Path -Leaf $FilePath)"
    } else {
        Write-Host "Error uploading file."
    }
} catch {
    Write-Host "An error occurred: $_"
}

 ```

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
