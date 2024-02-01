# Project README


## Note

Do not clone this repository if you intend to complete the project yourself. Instead, follow the provided instructions to set up the project with your AWS account and CodeCommit or GitHub repository.

For troubleshooting and additional details, refer to the AWS documentation.

**Important:** Follow all security best practices, and keep your AWS credentials secure.


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



# Module 2: Manage Users

In this module, you will create an Amazon Cognito user pool to manage user accounts for your web application. Users visiting your website will register a new user account, providing an email address and password. Amazon Cognito will handle the registration process, sending a confirmation email with a verification code to the user.

## Create an Amazon Cognito User Pool and Integrate an App

1. **Create User Pool:**
   - In the Amazon Cognito console, choose **Create user pool.**
   - Configure sign-in options, selecting User name in the Cognito user pool sign-in options.
   - Keep default settings for other options and proceed.

2. **Configure Security Requirements:**
   - On the Configure security requirements page, keep the Password policy mode as Cognito defaults.
   - Configure multi-factor authentication (MFA) if desired, or choose No MFA.
   - Proceed to the next step.

3. **Configure Sign-up Experience:**
   - Keep default settings on the Configure sign-up experience page.
   - Proceed to the next step.

4. **Configure Message Delivery:**
   - Confirm that Send email with Amazon SES - Recommended is selected.
   - In the FROM email address field, select a verified email address with Amazon SES.
   - Proceed to the next step.

5. **Integrate Your App:**
   - Name your user pool as "WildRydes."
   - Under Initial app client, name the app client "WildRydesWebApp" with default settings.
   - Create the user pool.

6. **Copy User Pool ID and App Client ID:**
   - On the User pools page, select the User pool name and copy the User Pool ID.
   - Navigate to the App Integration tab and copy the Client ID from the App clients and analytics section.
   - Save both IDs in a secure location.

## Update the Website Config File

Update the `js/config.js` file with the User Pool ID, App Client ID, and Region.

1. Open the `wildryde-site/js/config.js` file in a text editor.
2. Update the `cognito` section with the User Pool ID, App Client ID, and Region.
3. Save the modified file.

The updated `config.js` file should look like the following:

```javascript
window._config = {
    cognito: {
        userPoolId: 'your-user-pool-id',
        userPoolClientId: 'your-app-client-id',
        region: 'your-aws-region'
    },
    api: {
        invokeUrl: ''
    }
};
```

4. Add, commit, and push the file to your Git repository to trigger automatic deployment to Amplify Console.

## Make Sure Everything Works

1. Navigate to the `wildrydes-site` folder on your local machine.
2. Open `/register.html` or choose the "Giddy Up!" button on the homepage (`index.html`).
3. Complete the registration form and choose "Let's Ryde."
4. Confirm your new user using either the verification email or manually through the Cognito console.
5. Visit `/signin.html` and log in using the registered email and password.
6. After successful login, you should be redirected to `/ride.html`. Note the auth token for use in the next module.

**Important:** Ensure to copy and save the auth token for creating the Amazon Cognito user pool authorizer in the next module.


# Module 3: Serverless Service Backend

In this module, you will use AWS Lambda and Amazon DynamoDB to create a backend process for handling requests in your web application. The browser application deployed in the first module allows users to request a unicorn, and this module will implement a Lambda function to fulfill those requests. The function selects a unicorn from a fleet, records the request in a DynamoDB table, and responds to the frontend application with details about the dispatched unicorn.

## Create an Amazon DynamoDB Table

1. In the Amazon DynamoDB console, choose **Create table.**
2. Enter **Rides** for the Table name.
3. For the Partition key, enter **RideId** and select **String** for the key type.
4. In the Table settings section, ensure **Default settings** is selected, and choose **Create table.**
5. Wait for the table to become **Active**.

## Create an IAM Role for Your Lambda Function

Create an IAM role to grant your Lambda function permissions to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.

1. In the IAM console, select **Roles** and choose **Create Role.**
2. In the Trusted Entity Type section, select **AWS service.** For Use case, select **Lambda,** then choose **Next.**
3. Enter **AWSLambdaBasicExecutionRole** in the filter text box and press **Enter.**
4. Select the checkbox next to **AWSLambdaBasicExecutionRole** policy name and choose **Next.**
5. Enter **WildRydesLambda** for the Role Name and choose **Create Role.**
6. On the Permissions tab, under **Add permissions,** choose **Create Inline Policy.**
7. Type **DynamoDB** into the search bar, and select **DynamoDB** when it appears. Choose **Select actions.**
8. Type **PutItem** into the search bar, select the checkbox next to **PutItem,** and choose **Add ARN.**
9. Paste the ARN of the table you created in DynamoDB (from Step 5 in the previous section) and choose **Add ARNs.**
10. Choose **Next.**
11. Enter **DynamoDBWriteAccess** for the policy name and choose **Create policy.**

## Create a Lambda Function for Handling Requests

Create a Lambda function named **RequestUnicorn** using the AWS Lambda console. Use the provided `requestUnicorn.js` example implementation for your function code.

1. From the AWS Lambda console, choose **Create a function.**
2. Keep the default **Author from scratch** card selected.
3. Enter **RequestUnicorn** in the **Function name** field.
4. Select **Node.js 16.x** for the **Runtime.**
5. Select **Use an existing role** from the **Change default execution role** dropdown.
6. Select **WildRydesLambda** from the **Existing Role** dropdown.
7. Choose **Create function.**
8. Scroll down to the **Code source** section and replace the existing code in the `index.js` code editor with the contents of `requestUnicorn.js`.
9. Choose **Deploy.**

## Validate that it Works

Test the function using the AWS Lambda console.

1. In the **RequestUnicorn** function, choose **Test** in the **Code source** section, and select **Configure test event** from the dropdown.
2. Keep the **Create new event** default selection.
3. Enter **TestRequestEvent** in the **Event name** field.
4. Copy and paste the provided test event into the **Event JSON** section.
5. Choose **Save.**
6. In the **Code source** section of your function, choose **Test** and select **TestRequestEvent** from the dropdown.
7. On the **Test** tab, choose **Test.**
8. Verify that the function result looks as expected.

```json
{
    "statusCode": 201,
    "body": "{\"RideId\":\"SvLnijIAtg6inAFUBRT+Fg==\",\"Unicorn\":{\"Name\":\"Rocinante\",\"Color\":\"Yellow\",\"Gender\":\"Female\"},\"Eta\":\"30 seconds\"}",
    "headers": {
        "Access-Control-Allow-Origin": "*"
    }
}
```

Now, you have successfully created a Lambda function to handle requests in your web application. In the next module, you'll add a REST API with API Gateway to invoke this function from the browser-based application.



