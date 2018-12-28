# aws-serverless-cfn-template

AWS CloudFormation template creating a Serverless application using AWS Lambda and API Gateway

## Description

This AWS CloudFormation template creates a AWS Lambda Serverless application which outputs generic JSON data sitting behind AWS API Gateway to handle a RESTful HTTP call via curl or a web browser. This Serverless application example can be useful when natively creating Serverless endpoints through CloudFormation. Other tools exist to do similar like serverless.com or Zappa which also allow for more end to end development built in. This however is meant to be as simplistic as possible just leveraging CloudFormation.

_***note AWS S3, API Gateway, and Lambda will incur costs**_

* [S3 pricing](https://aws.amazon.com/s3/pricing/)
* [API Gateway pricing](https://aws.amazon.com/api-gateway/pricing/)
* [Lambda pricing](https://aws.amazon.com/lambda/pricing/)

## Components

### serverless-cft.yaml

This CloudFormation template written in YAML format states where the Lambda application exists, what it is named along with some configuration settings such as memory allocated, timeout for the application, runtime interpreter, and also creates an API Gateway endpoint with the proper handling of HTTP response codes in concert with the Lambda application.

**Things to change or note**

* Change your S3Bucket to the name of your bucket where you put the lambda.zip
* Change the role name in two places from "role/serverless" to the role name you created
* To test use the URL provided in the Outputs under TestURL in a browser

### lambda-app.zip

This is a compressed zip file of lambda-app.py which is a simple function which when called outputs hard coded sample JSON. Lambda reads this zip file and uses the designated interpreter to execute it. In this example we are using Python as our language and the Python interpreter.

## IAM Permissions

There are two IAM permissions outlined below, one is a IAM User to execute the CloudFormation Template to create the Serverless application (API Gateway and Lambda), and the other for the Serverless application IAM Role to allow your application to execute.

### **IAM User** (To Execute the serverless-cft.yaml CloudFormation Template)

#### Permissions:

* AWSCloudFormationReadOnlyAccess
* AmazonAPIGatewayAdministrator
* AWSLambdaFullAccess
* AmazonS3FullAccess

*Note you can make these permissions more strict depending on your needs*

### **IAM Role** (For the Serverless application)

#### Permissions:

* AmazonAPIGatewayInvokeFullAccess
* AWSLambdaExecute
* AWSLambdaRole

#### Trust relationships:

```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "apigateway.amazonaws.com",
          "lambda.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

```
