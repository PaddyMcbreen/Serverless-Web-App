# Project README

## Project Overview

This repository contains the code for a web application that leverages various AWS services such as AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. The application's architecture allows for static web hosting with continuous deployment, creating a dynamic user experience.

## Project Setup

Before proceeding, ensure you have the following:

- An AWS Account
- Visual Studio (or alternative)

### Application Architecture

The application architecture is built upon AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console handles continuous deployment and hosting of static web resources, while JavaScript executed in the browser interacts with a public backend API built using Lambda and API Gateway. Amazon Cognito ensures user management and authentication, and DynamoDB serves as the persistence layer.

## Module 1: Static Web Hosting with Continuous Deployment

To set up the project, follow these steps:

1. **Select a Region:**
   - Choose an AWS Region supporting all required services (AWS Amplify, AWS CodeCommit, Amazon Cognito, AWS Lambda, Amazon API Gateway, and Amazon DynamoDB). Recommended regions include:
     - US East (N. Virginia)
     - US East (Ohio)
     - US West (Oregon)
     - EU (Frankfurt)
     - EU (Ireland)
     - EU (London)
     - Asia Pacific (Tokyo)
     - Asia Pacific (Seoul)
     - Asia Pacific (Sydney)
     - Asia Pacific (Mumbai)
   - Select your Region from the dropdown in the upper right corner of the AWS Management Console.

2. **Create a Git Repository:**
   - Choose between AWS CodeCommit or GitHub for source code management.
   - If using CodeCommit:
     - Install AWS CLI if not installed.
     - Open AWS CodeCommit console.
     - Create a repository named "wildrydes-site."
     - Set up an IAM user with Git credentials.
     - Clone the repository locally using the provided HTTPS URL.

3. **Set Up Git Config:**
   - Configure Git credentials and helper by following the provided commands in the terminal.

4. **Clone Repository:**
   - Navigate to the CodeCommit console.
   - Select the "wildrydes-site" repository.
   - Clone the repository using HTTPS URL in the terminal.

5. **Populate the Git Repository:**
   - Change directory into the cloned repository.
   - Copy static files from the provided S3 bucket using AWS CLI commands.
   - Add, commit, and push the Git files.

6. **Enable Web Hosting with AWS Amplify Console:**
   - Launch the AWS Amplify console.
   - Follow the steps to set up Amplify Hosting, linking to the CodeCommit repository.
   - Allow automatic deployment of files in the project root directory.

7. **Modify Your Site:**
   - Make changes to the local files, commit, and push to the Git repository.
   - AWS Amplify Console will automatically rebuild and redeploy the app.

## Note

Do not clone this repository if you intend to complete the project yourself. Instead, follow the provided instructions to set up the project with your AWS account and CodeCommit or GitHub repository.

For troubleshooting and additional details, refer to the AWS documentation.

**Important:** Follow all security best practices, and keep your AWS credentials secure.