Notes:

If you want to complete this project yourself, do not clone this repository!
Follow the instructions below step by step, the code in this repo is a publicly accessible S3 bucket that was pulled. You will be guided through this in the instructions below.




To create and use this project, you will need:

- An AWS Account
- Visual Studio (or alternative)




Application architecture


The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.



Module 1: Static Web Hosting with Continuous Deployment

To get started, you will configure AWS Amplify to host the static resources for your web application with continuous deployment built in

In this module, you will configure AWS Amplify to host the static resources for your web application with continuous deployment built in. The Amplify Console provides a git-based workflow for continuous deployment and hosting of full-stack web apps. In subsequent modules, you will add dynamic functionality to these pages using JavaScript to call remote RESTful APIs built with AWS Lambda and Amazon API Gateway.



Select a Region:

This web application can be deployed in any AWS Region that supports all the services used in this application, which include AWS Amplify, AWS CodeCommit, Amazon Cognito, AWS Lambda, Amazon API Gateway, and Amazon DynamoDB.

You can refer to the AWS Regional Services List to see which Regions have the supported services. Among the supported Regions you can choose are:

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

Select your Region from the dropdown in the upper right corner of the AWS Management Console.



Create a Git Repository


You have two options to manage the source code for this module: AWS CodeCommit (included in the AWS Free Tier) or GitHub. In this tutorial, we will use CodeCommit to store our application code, but you can do the same by creating a repository on GitHub.

1. If you have never configured AWS CLI on your local machine, open a terminal window to install AWS CLI. The installation instructions vary depending on what operating system you're using. If you already have AWS CLI installed and configured, skip to Step 2.
2. Open the AWS CodeCommit console.
3. Choose Create Repository.
4. Enter wildrydes-site for the Repository name.
5. Choose Create.


Once repository is created, set up an IAM user with Git credentials in the IAM console. Follow the instructions for Step 1 through Step 3 on the Setup for HTTPS users using Git credentials page of the AWS CodeCommit User Guide. 

Important Note: When setting up your user in the IAM console, you will need to set up and save two sets of credentials to refer back to.
1. You must create Access keys in the IAM > Security Credentials tab. Download the Access Key and Secret Access Key IDs or copy and save them in a secure location.
2. You must also generate HTTPS Git credentials for AWS CodeCommit. Download or save these generated credentials as well.
3. In the terminal window you used to install AWS CLI, enter the command: aws configure
4. Enter the AWS Access Key ID and Secret Access Key you created in Step 6. 
5. For Default region name enter the Region you initially selected to create your CodeCommit repository in.
6. Leave Default output format blank, and press enter. 
The following code block is an example of what you will see in your terminal window.

```
% aws configure
AWS Access Key ID [****************]: #####################
AWS Secret Access Key [****************]: ###################
Default region name [us-east-1]: us-east-1
Default output format [None]:
```

 11. Set up the git config credential helper in the terminal window.

- If you have a Linux, macOS or UNIX machine, see the Step 3: Set up the credential helper instructions for Linux, macOS, or UNIX.
- If you have a Windows machine see the Step 3: Set up the credential helper instructions for Windows.

```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

12. Navigate back to the AWS CodeCommit console and select the wildrydes-site repository.

13. Select Clone HTTPS from the Clone URL dropdown to copy the HTTPS URL. 

 14. From your terminal window run git clone and paste the HTTPS URL of the repository. 

The following code block is an example of what you will see in your terminal window:

```
$ git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
Cloning into ‘wildrydes-site’...
Username for ‘https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site’: Enter the HTTPS Git credentials for AWS CodeCommit username you generated in Step 6
Password for ‘https://username@git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site’: Enter the HTTPS Git credentials for AWS CodeCommit password you generated in Step 6
warning: You appear to have cloned an empty repository.
```


There will be a warning that you appear to have cloned an empty repository, this is expected. 

For common fixes to errors thrown when cloning the repository, see Troubleshooting the credential helper and HTTPS connections to AWS CodeCommit. 




Populate the Git Repository



Once you've used either AWS CodeCommit or GitHub.com to create your git repository and clone it locally, you need to copy the website content from an existing, publicly-accessible S3 bucket associated with this tutorial and add the content to your repository.

1. Change directory into your repository and copy the static files from S3 using the following commands (make sure you change the Region in the following command to copy the files from the S3 bucket to the Region you selected at the beginning of this tutorial):

```
cd wildrydes-site (command to change to your repo)

aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ --recursive (command to pull the S3 Bucket)
```

2. Add, commit, and push the git files. 

The following code block is an example of what you will see in your terminal window:

```
$ git add .
$ git commit -m "new files"
$ git push

Counting objects: 95, done.
Compressing objects: 100% (94/94), done.
Writing objects: 100% (95/95), 9.44 MiB | 14.87 MiB/s, done.
Total 95 (delta 2), reused 0 (delta 0)
To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
* [new branch] master -> master
```





Enable Web Hosting with the AWS Amplify Console


Next you'll use the AWS Amplify Console to deploy the website you've just committed to git. The Amplify Console takes care of the work of setting up a place to store your static web application code and provides a number of helpful capabilities to simplify both the lifecycle of that application as well as enable best practices.

1. Launch the AWS Amplify console. 
2. Choose Get Started. 
3. Under the Amplify Hosting Host your web app header, choose Get Started. 
4. On the Get started with Amplify Hosting page, select AWS CodeCommit and choose Continue.
5. On the Add repository branch step, select wildrydes-site from the Select a repository dropdown.
6. If you used GitHub, you'll need to authorize AWS Amplify to your GitHub account.
7. In the Branch dropdown select master and choose Next. 

  8. On the Build settings page, leave all the defaults, select Allow AWS Amplify to automatically deploy all files hosted in your project root directory and choose Next.

9. On the Review page select Save and deploy.

10. The process takes a couple of minutes for Amplify Console to create the necessary resources and to deploy your code.

Once completed, select the site image, or the link underneath the thumbnail to launch your Wild Rydes site. If you select the link for master you'll see the build and deployment details related to your branch. 



Modify your Site


The AWS Amplify console will rebuild and redeploy the app when it detects changes to the connected repository. Make a change to the main page to test out this process.

1. On your local machine, navigate to the the wildrydes-site folder and open the index.html file in a text editor of your choice. 
2. Modify the title line with the following text: <title>Wild Rydes - Rydes of the Future!</title>
3. Save the file. 
4. In your terminal window, add, commit your change, and push the change to the git repository again. Amplify Console will begin to build the site again soon after it notices the update to the repository. It will happen pretty quickly! Head back to the AWS Amplify console to watch the process.

The following code block is an example of what you will see in your terminal window: 
```
$ git add index.html 
$ git commit -m "updated title"
[master dfec2e5] updated title
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git push
Counting objects: 3, done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: processing 
To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
   2e9f540..dfec2e5  master -> master
```


    5. Once Amplify has completed the re-deployment, re-open the Wild Rydes site and notice the tab title change.




Module 2: Manage Users

You will create an Amazon Cognito user pool to manage your users' accounts

When users visit your website they will first register a new user account. For the purposes of this workshop we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console with a fake email addresses for testing.

After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.



Create an Amazon Cognito User Pool and Integrate an App with your User Pool


- Amazon Cognito provides two different mechanisms for authenticating users. You can use Cognito User Pools to add sign-up and sign-in functionality to your application or use Cognito Identity Pools to authenticate users through social identity providers such as Facebook, Twitter, or Amazon, with SAML identity solutions, or by using your own identity system. For this module you'll use a user pool as the backend for the provided registration and sign-in pages.
  
  In the Amazon Cognito console, choose Create user pool.
- On the Configure sign-in experience page, in the Cognito user pool sign-in options section, select User name. Keep the defaults for the other settings, such as Provider types and do not make any User name requirements selections. Choose Next.
- On the Configure security requirements page, keep the Password policy mode as Cognito defaults. You can choose to configure multi-factor authentication (MFA) or choose No MFA and keep other configurations as default. Choose Next.
- On the Configure sign-up experience page, keep everything as default. Choose Next.
- On the Configure message delivery page, for Email provider, confirm that Send email with Amazon SES - Recommended is selected. In the FROM email address field, select an email address that you have verified with Amazon SES, following the instructions in Verifying an email address identity in the Amazon Simple Email Service Developer Guide.

Note: If you don't see the verified email address populating in the dropdown, ensure that you have created a verified email address in the same Region you selected at the beginning of the tutorial. 

-  On the Integrate your app page, name your user pool: WildRydes. Under Initial app client, name the app client: WildRydesWebApp and keep the other settings as default.
- On the Review and create page, choose Create user pool.
- On the User pools page, select the User pool name to view detailed information about the user pool you created. Copy the User Pool ID in the User pool overview section and save it in a secure location on your local machine. 
- Select the App Integration tab and copy and save the Client ID in the App clients and analytics section of your newly created user pool.



Update the Website Config File


The js/config.js file contains settings for the user pool ID, app client ID and Region. Update this file with the settings from the user pool and app you created in the previous steps and upload the file back to your bucket.

1. From your local machine, open the wildryde-site/js/config.js file in a text editor of your choice.
2. Update the cognito section of the file with the correct values for the User pool ID and App Client ID you saved in Steps 8 and 9 in the previous section. The userPoolID is the User pool ID from the User pool overview section, and the userPoolClientID is the App Client ID from the App Integration > App clients and analytics section of Amazon Cognito. 
3. The value for region should be the AWS Region code where you created your user pool. For example, us-east-1 for the N. Virginia Region, or us-west-2 for the Oregon Region. If you're not sure which code to use, you can look at the Pool ARN value on the User pool overview. The Region code is the part of the ARN immediately after arn:aws:cognito-idp:.

The updated config.js file should look like the following code. Note that the actual values for your file will be different:

```
window._config = {
    cognito: {
        userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
        userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
        region: 'us-west-2' // e.g. us-east-2
    },
    api: {
        invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod',
    }
};
```


    4. Save the modified file.

    5. In your terminal window, add, commit, and push the file to your Git repository to have it automatically deploy to Amplify Console.



Make Sure Everything Works


1. In a Finder window or Windows File Explorer, navigate to the wildrydes-site folder you copied to your local machine in Module 1. 
2. Open /register.html, or choose the Giddy Up! button on the homepage (index.html page) of your site.
3. Complete the registration form and choose Let's Ryde. You can use your own email or enter a fake email. Make sure to choose a password that contains at least one upper-case letter, a number, and a special character. Don't forget the password you entered for later. You should see an alert that confirms that your user has been created.
4. Confirm your new user using one of the two following methods:
	1. If you used an email address you control, you can complete the account verification process by visiting /verify.html under your website domain and entering the verification code that is emailed to you. Please note, the verification email may end up in your spam folder. For real deployments we recommend configuring your user pool to use Amazon Simple Email Service to send emails from a domain you own.
	2. If you used a dummy email address, you must confirm the user manually through the Cognito console.
		1. In the Amazon Cognito console, select the WildRydes user pool.
		2. In the Users tab, you should see a user corresponding to the email address that you submitted through the registration page. Choose that username to view the user detail page.
		3. In the Actions dropdown, select Confirm account to finalize the account creation process.
		4. In the Confirm account for user pop-up, choose Confirm.
5. After confirming the new user using either the /verify.html page or the Cognito console, visit /signin.html and log in using the email address and password you entered during the registration step.
6. If successful you should be redirected to /ride.html. You should see a notification that the API is not configured.
   
   Important: Copy and save the auth token in order to create the Amazon Cognito user pool authorizer in the next module.



Module 3: Serverless Service Backend

You will use AWS Lambda and Amazon DynamoDB to build a backend process for handling requests for your web application

In this module, you will use AWS Lambda and Amazon DynamoDB to build a backend process for handling requests for your web application. The browser application that you deployed in the first module allows users to request that a unicorn be sent to a location of their choice. To fulfill those requests, the JavaScript running in the browser will need to invoke a service running in the cloud.

You will implement a Lambda function that will be invoked each time a user requests a unicorn. The function will select a unicorn from the fleet, record the request in a DynamoDB table, and then respond to the frontend application with details about the unicorn being dispatched.

The function is invoked from the browser using Amazon API Gateway. You'll implement that connection in the next module. For this module, you will just test your function in isolation.



 Create an Amazon DynamoDB Table


- Use the Amazon DynamoDB console to create a new DynamoDB table. 
  
  In the Amazon DynamoDB console, choose Create table.For the Table name, enter Rides. This field is case sensitive.
- For the Partition key, enter RideId and select String for the key type. This field is case sensitive.
- In the Table settings section, ensure Default settings is selected, and choose Create table. 
- On the Tables page, wait for your table creation to complete. Once it is completed, the status will say Active. Select your table name.
- In the Overview tab > General Information section of your new table and choose Additional info. Copy the ARN. You will use this in the next section.


Create an IAM Role for your Lambda Function


Every Lambda function has an IAM role associated with it. This role defines what other AWS services the function is allowed to interact with. For the purposes of this tutorial, you'll need to create an IAM role that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.

- In the IAM console, select Roles in the left navigation pane and then choose Create Role. 
- In the Trusted Entity Type section, select AWS service. For Use case, select Lambda, then choose Next. 
  
  Note: Selecting a role type automatically creates a trust policy for your role that allows AWS services to assume this role on your behalf. If you are creating this role using the CLI, AWS CloudFormation, or another mechanism, specify a trust policy directly.

- Enter AWSLambdaBasicExecutionRole in the filter text box and press Enter. 
- Select the checkbox next to the AWSLambdaBasicExecutionRole policy name and choose Next.
- Enter WildRydesLambda for the Role Name. Keep the default settings for the other parameters.Choose Create Role.
- In the filter box on the Roles page type WildRydesLambda and select the name of the role you just created.
- On the Permissions tab, under Add permissions, choose Create Inline Policy.
- In the Select a service section, type DynamoDB into the search bar, and select DynamoDB when it appears. Choose Select actions.
- In the Actions allowed section, type PutItem into the search bar and select the checkbox next to PutItem when it appears.
- In the Resources section, with the Specific option selected, choose the Add ARN link.
- Select the Text tab. Paste the ARN of the table you created in DynamoDB (Step 6 in the previous section), and choose Add ARNs.
- Choose Next.
- Enter DynamoDBWriteAccess for the policy name and choose Create policy.



Create a Lambda Function for Handling Requests


AWS Lambda will run your code in response to events such as an HTTP request. In this step you'll build the core function that will process API requests from the web application to dispatch a unicorn. In the next module you'll use Amazon API Gateway to create a RESTful API that will expose an HTTP endpoint that can be invoked from your users' browsers. You'll then connect the Lambda function you create in this step to that API in order to create a fully functional backend for your web application.

Use the AWS Lambda console to create a new Lambda function called RequestUnicorn that will process the API requests. Use the following requestUnicorn.js example implementation for your function code. Just copy and paste from that file into the AWS Lambda console's editor.

Make sure to configure your function to use the WildRydesLambda IAM role you created in the previous section.
1. From the AWS Lambda console, choose Create a function.
2. Keep the default Author from scratch card selected.
3. Enter RequestUnicorn in the Function name field.
4. Select Node.js 16.x for the Runtime (newer versions of Node.js will not work in this tutorial).
5. Select Use an existing role from the Change default execution role dropdown.
6. Select WildRydesLambda from the Existing Role dropdown.
7. Click on Create function.
8. Scroll down to the Code source section and replace the existing code in the index.js code editor with the contents of requestUnicorn.js. The following code block displays the requestUnicorn.js file. Copy and paste this code into the index.js tab of the code editor.

```
const randomBytes = require('crypto').randomBytes;
const AWS = require('aws-sdk');
const ddb = new AWS.DynamoDB.DocumentClient();

const fleet = [
    {
        Name: 'Angel',
        Color: 'White',
        Gender: 'Female',
    },
    {
        Name: 'Gil',
        Color: 'White',
        Gender: 'Male',
    },
    {
        Name: 'Rocinante',
        Color: 'Yellow',
        Gender: 'Female',
    },
];

exports.handler = (event, context, callback) => {
    if (!event.requestContext.authorizer) {
      errorResponse('Authorization not configured', context.awsRequestId, callback);
      return;
    }

    const rideId = toUrlString(randomBytes(16));
    console.log('Received event (', rideId, '): ', event);

    // Because we're using a Cognito User Pools authorizer, all of the claims
    // included in the authentication token are provided in the request context.
    // This includes the username as well as other attributes.
    const username = event.requestContext.authorizer.claims['cognito:username'];

    // The body field of the event in a proxy integration is a raw string.
    // In order to extract meaningful values, we need to first parse this string
    // into an object. A more robust implementation might inspect the Content-Type
    // header first and use a different parsing strategy based on that value.
    const requestBody = JSON.parse(event.body);

    const pickupLocation = requestBody.PickupLocation;

    const unicorn = findUnicorn(pickupLocation);

    recordRide(rideId, username, unicorn).then(() => {
        // You can use the callback function to provide a return value from your Node.js
        // Lambda functions. The first parameter is used for failed invocations. The
        // second parameter specifies the result data of the invocation.

        // Because this Lambda function is called by an API Gateway proxy integration
        // the result object must use the following structure.
        callback(null, {
            statusCode: 201,
            body: JSON.stringify({
                RideId: rideId,
                Unicorn: unicorn,
                Eta: '30 seconds',
                Rider: username,
            }),
            headers: {
                'Access-Control-Allow-Origin': '*',
            },
        });
    }).catch((err) => {
        console.error(err);

        // If there is an error during processing, catch it and return
        // from the Lambda function successfully. Specify a 500 HTTP status
        // code and provide an error message in the body. This will provide a
        // more meaningful error response to the end client.
        errorResponse(err.message, context.awsRequestId, callback)
    });
};

// This is where you would implement logic to find the optimal unicorn for
// this ride (possibly invoking another Lambda function as a microservice.)
// For simplicity, we'll just pick a unicorn at random.
function findUnicorn(pickupLocation) {
    console.log('Finding unicorn for ', pickupLocation.Latitude, ', ', pickupLocation.Longitude);
    return fleet[Math.floor(Math.random() * fleet.length)];
}

function recordRide(rideId, username, unicorn) {
    return ddb.put({
        TableName: 'Rides',
        Item: {
            RideId: rideId,
            User: username,
            Unicorn: unicorn,
            RequestTime: new Date().toISOString(),
        },
    }).promise();
}

function toUrlString(buffer) {
    return buffer.toString('base64')
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=/g, '');
}

function errorResponse(errorMessage, awsRequestId, callback) {
  callback(null, {
    statusCode: 500,
    body: JSON.stringify({
      Error: errorMessage,
      Reference: awsRequestId,
    }),
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  });
}
```

9. Choose Deploy.



Validate that it Works


For this module you will test the function that you built using the AWS Lambda console. In the next module you will add a REST API with API Gateway so you can invoke your function from the browser-based application that you deployed in the first module.

1. In the RequestUnicorn function you built in the previous section, choose Test in the Code source section, and select Configure test event from the dropdown.
2. Keep the Create new event default selection.
3. Enter TestRequestEvent in the Event name field.
4. Copy and paste the following test event into the Event JSON section:

```
{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}
```


   5. Choose Save.

    6. In the Code source section of your function, choose Test and select TestRequestEvent from the dropdown.

    7.  On the Test tab, choose Test.

    8. In the Executing function:succeeded message that appears, expand the Details dropdown.

    9. Verify that the function result looks like the following:

```
{
    "statusCode": 201,
    "body": "{\"RideId\":\"SvLnijIAtg6inAFUBRT+Fg==\",\"Unicorn\":{\"Name\":\"Rocinante\",\"Color\":\"Yellow\",\"Gender\":\"Female\"},\"Eta\":\"30 seconds\"}",
    "headers": {
        "Access-Control-Allow-Origin": "*"
    }
}
```





Module 4: Deploy a RESTful API

You will use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API

In this module, you will use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API. This API will be accessible on the public Internet. It will be secured using the Amazon Cognito user pool you created in the previous module. Using this configuration, you will then turn your statically hosted website into a dynamic web application by adding client-side JavaScript that makes AJAX calls to the exposed APIs.


Create a new REST API


1. In the Amazon API Gateway console, select APIs in the left navigation pane. 
2. Choose Build under REST API. 
3. In the Choose the protocol section, select REST. 
4. In the Create new API section, select New API.
5. In the Settings section, enter WildRydes for the API Name and select Edge optimized in the Endpoint Type dropdown.
   
   Note: Use edge-optimized endpoint types for public services being accessed from the Internet. Regional endpoints are typically used for APIs that are accessed primarily from within the same AWS Region.
6. Choose Create API.



Create a new Authoriser


You must create an Amazon Cognito User Pools Authorizer. Amazon API Gateway uses JSON web tokens (JWT), which are returned by the Amazon Cognito User Pool (created in Module 2) to authenticate the API calls. In this section, we will be creating an Authorizer for the API, so we can make use of the user pool.

Use the following steps to configure the Authorizer in the Amazon API Gateway console:

1. In the left navigation pane of the WildRydes API you just created, select Authorizers.
2. Choose Create New Authorizer. 
3. Enter WildRydes into the Authorizer Name field.
4. Select Cognito as the Type. 
5. Under Cognito User Pool, in the Region drop-down, select the same Region you have been using for the rest of the tutorial. Enter WildRydes in the Cognito User Pool name field. 
6. Enter Authorization in the Token Source section. 
7. Choose Create.
8. To verify the authorizer configuration, select Test. 
9. Paste the Authorization Token copied from the ride.html webpage in the Validate your implementation section of Module 2 into the Authorization (header) field, and verify that the HTTP status Response code is 200. 


Create a new Resource and Method


- In this section you will create a new resource within your API. Then create a POST method for that resource and configure it to use a Lambda proxy integration backed by the RequestUnicorn function you created in the first step of this module.
  
  In the left navigation pane of your WildRydes API, select Resources.
- From the Actions dropdown, select Create Resource.
- Enter ride as the Resource Name, which will automatically create the Resource Path /ride.
- Select the checkbox for Enable API Gateway CORS.
- Choose Create Resource.
- With the newly created /ride resource selected, from the Actions dropdown select Create Method.
- Select POST from the new dropdown that appears under OPTIONS, then select the checkmark icon.
- Select Lambda Function for the Integration type.
- Select the checkbox for Use Lambda Proxy integration.
- Select the same Region you have been using throughout the tutorial for Lambda Region.
- Enter RequestUnicorn for Lambda Function.
- Choose Save.
  
  Note: If you get an error that your function does not exist, check that the region you selected matches the one you used in the previous modules.

- When prompted to give Amazon API Gateway permission to invoke your function, choose OK.
- Select the Method Request card.
- Choose the pencil icon next to Authorization.
- Select the WildRydes Cognito user pool authorizer from the drop-down list, and select the checkmark icon.



Deploy your API


- In this section you will deploy your API from the Amazon API Gateway console. 

- In the Actions drop-down list select Deploy API.
- Select [New Stage] in the Deployment stage drop-down list.
- Enter prod for the Stage Name.
- Choose Deploy.
- Copy the Invoke URL. You will use it in the next section.



Update the Website Config


In this step you will update the /js/config.js file in your website deployment to include the Invoke URL of the stage you just created. You will copy the Invoke URL directly from the top of the stage editor page on the Amazon API Gateway console and paste it into the invokeUrl key of your site's config.js file. Your config file will still contain the updates you made in the previous module for your Amazon Cognito userPoolID, userPoolClientID, and region.

1. On your local machine, navigate to the js folder, and open the config.js file in a text editor of your choice
2. Paste the Invoke URL you copied from the Amazon API Gateway console in the previous section into the invokeUrl value of the config.js file. 
3. Save the file.

See the following example of a completed config.js file. Note, the actual values in your file will be different.
```
window._config = {

    cognito: {

        userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb         

        userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv

        region: 'us-west-2' // e.g. us-east-2 

    }, 

    api: { 

        invokeUrl: 'https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod, 

    } 

};
```

    4. Add, commit, and push the updated config.js file to your Git repository to have it automatically deploy to Amplify Console.




Verify everything Works Correctly


Note: It is possible that you will see a delay between updating the config.js file in your S3 bucket and when the updated content is visible in your browser. You should also ensure that you clear your browser cache before executing the following steps.

1. Update the ArcGIS JS version from 4.3 to 4.6 (newer versions will not work in this tutorial) in the ride.html file as:

```
<script src="https://js.arcgis.com/4.6/"></script>
 <link rel="stylesheet" href="https://js.arcgis.com/4.6/esri/css/main.css">
```


An example of a complete ride.htmlfile is included below. Note, some values in your file may be different.

```
<div id="noApiMessage" class="configMessage" style="display: none;">
        <div class="backdrop"></div>
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3 class="panel-title">Successfully Authenticated!</h3>
            </div>
            <div class="panel-body">
                <p>This page is not functional yet because there is no API invoke URL configured in <a href="/js/config.js">/js/config.js</a>. You'll configure this in Module 3.</p>
                <p>In the meantime, if you'd like to test the Amazon Cognito user pool authorizer for your API, use the auth token below:</p>
                <textarea class="authToken"></textarea>
            </div>
        </div>
    </div>

    <div id="noCognitoMessage" class="configMessage" style="display: none;">
        <div class="backdrop"></div>
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3 class="panel-title">No Cognito User Pool Configured</h3>
            </div>
            <div class="panel-body">
                <p>There is no user pool configured in <a href="/js/config.js">/js/config.js</a>. You'll configure this in Module 2 of the workshop.</p>
            </div>
        </div>
    </div>

    <div id="main">
        <div id="map">
        </div>
    </div>

    <div id="authTokenModal" class="modal fade" tabindex="-1" role="dialog" aria-labelledby="authToken">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                    <h4 class="modal-title" id="myModalLabel">Your Auth Token</h4>
                </div>
                <div class="modal-body">
                    <textarea class="authToken"></textarea>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>


    <script src="js/vendor/jquery-3.1.0.js"></script>
    <script src="js/vendor/bootstrap.min.js"></script>
    <script src="js/vendor/aws-cognito-sdk.min.js"></script>
    <script src="js/vendor/amazon-cognito-identity.min.js"></script>
    <script src="https://js.arcgis.com/4.6/"></script>
    <script src="js/config.js"></script>
    <script src="js/cognito-auth.js"></script>
    <script src="js/esri-map.js"></script>
    <script src="js/ride.js"></script>
</body>

</html>
```

    2. Save the modified file. Add, commit, and git push it to your Git repository to have it automatically deploy to AWS Amplify console.

    3. Visit /ride.html under your website domain.

    4. If you are redirected to the ArcGIS sign-in page, sign in with the user credentials you created previously in the Introduction section as a prerequisite of this tutorial.

    5. After the map has loaded, click anywhere on the map to set a pickup location.

    6. Choose Request Unicorn. You should see a notification in the right sidebar that a unicorn is on its way and then see a unicorn icon fly to your pickup location.