# Individual Project 4: Serverless Data Engineering Pipeline

## Objectvices of Project

In this project, a serverless architecture has been built using various AWS services such as Lambda, DynamoDB, SQS, S3, and AWS Comprehend. The DynamoDB table contains entries of company names, which are read by the architecture every minute. After reading the data, the architecture generates Wikipedia snippets for each company, performs sentiment analysis, extracts key phrases and entities from the snippets, and writes the results to an S3 bucket.

The workflow comprises of two Lambda functions. The first function reads data from the DynamoDB table and puts messages into an SQS queue, while the second function reads from the SQS queue, generates Wikipedia snippets, performs analysis using AWS Comprehend, and writes the results to an S3 bucket.


## Pipeline Architecture

<img width="701" alt="image" src="https://user-images.githubusercontent.com/104106034/231558580-ee32b89c-0bbc-4a91-9b53-c03e0bb0e079.png">

## Reference

Source code: https://github.com/noahgift/awslambda



## Steps of building the project from scratch

### 1. Create Cloud9 working environment, DynamoDB table, SQS queue, and S3 bucket

- On AWS Cloud9 console, open the IDE of your working environment, create a new directory for this project.
- On AWS DynamoDB console, create the new table with the name 'fang' or whatever you want
- Add "Google", "Facebook", or whatever you want to add in the created table
- On AWS SQS console, create a standard queue called 'ProducerSQS' (or whatever your want)
- Open AWS S3 console, create a bucket called 'fangsentiment'(or whatever you want). But, the bucket name should be globally unique.


### 2. Create Producer Lambda function

The lambda function that reads from DynamoDB table and puts messages into SQS. It is triggered every minute by EventBridge

Created SAM application in 2 ways. You can choose whichever you want.

**Create SAM app using CLI**

AWS SAM (Serverless Application Model) is a framework for building serverless applications on AWS (Amazon Web Services). It is an open-source framework that extends AWS CloudFormation to provide a simplified way of defining the Amazon API Gateway APIs, AWS Lambda functions, and Amazon DynamoDB tables needed by your serverless application.

-  `sam init`
a command that initializes a new serverless application project by creating a sample template and directory structure.


- `sam build --use-container`

a command to build a deployment package for a serverless application using a Docker container.
When you run this command, SAM will package and build your code, including all the dependencies and libraries required by your application, inside a Docker container that matches the AWS Lambda environment. This ensures that the deployment package is consistent and that any native dependencies are compiled correctly.

<img width="1113" alt="image" src="https://user-images.githubusercontent.com/104106034/228634746-93721e6c-5da9-4b5c-9bd2-a4cc2751e147.png">


-  `sam validate`

a command that checks the syntax and semantics of the AWS SAM template and verifies that it is valid according to the AWS SAM specification. If the template is not valid, the command will return an error message indicating the issues with the template.

<img width="1126" alt="image" src="https://user-images.githubusercontent.com/104106034/228634806-1e27e311-9f1f-4cc4-8951-1373211e74ca.png">


- `sam local invoke`

a command to invoke a Lambda function locally on your development machine. It allows you to test your Lambda function code locally and get immediate feedback on its behavior.

<img width="1303" alt="image" src="https://user-images.githubusercontent.com/104106034/231564375-266c4d8d-4c35-4317-b8cb-975a254588bb.png">


- `sam deploy --guided`

a command that initiates a deployment of a serverless application using AWS Serverless Application Model (SAM).

The sam deploy command deploys an AWS SAM application, and the --guided flag is used to start a guided deployment process. The guided deployment process allows the user to interactively provide the required deployment parameters such as AWS region, stack name, and any other parameters defined in the AWS SAM template.

The sam deploy command deploys an AWS SAM application, and the --guided flag is used to start a guided deployment process. The guided deployment process allows the user to interactively provide the required deployment parameters such as AWS region, stack name, and any other parameters defined in the AWS SAM template.

<img width="1092" alt="image" src="https://user-images.githubusercontent.com/104106034/231564050-a5110d0c-775f-43a8-a1f0-df57a7be5ee0.png">

**Create SAM app using IDE**

<img width="737" alt="image" src="https://user-images.githubusercontent.com/104106034/231565091-e86ab6f2-dd5c-41b5-aea4-229a444e18c5.png">

To create a Lambda SAM Application in the Cloud9 IDE, first click on "AWS", then right-click on "Lambda", and select "Create Lambda SAM Application". From there, you can configure the settings for the SAM app by choosing Python 3.8 as the runtime, selecting the "AWS SAM Hello World" template for the SAM application, specifying the directory where you want to store the files, and giving your app a name such as 'saveToSQS' or any name you prefer.


### 3. Create Consumer Lambda function

The second lambda function is responsible for reading messages from an SQS queue and extracting the name of the corresponding company. It then generates Wikipedia snippets based on the name and performs sentiment analysis, key phrase detection, and entity detection on these snippets using the AWS Comprehend API. The analysis results are then written to an S3 bucket. This function is triggered by the SQS service.

- create SAM application named as 'checkSQS' or whatever you want in a similar way
- Add permission role with SQSaccess and adminaccess
- Add a SQS trigger that grabs message from SQS and send it for sentiment analysis

