# AWS SAM Feedback Bot

This project contains source code and supporting files for a serverless application that creates a Feedback Bot on AWS.

You can deploy it with the AWS Serverless Application Model (AWS SAM) command line interface (CLI) or with your own CI/CD pipeline. 

## Pre-requisites
* AWS SAM CLI - [Install the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html).
* AWS Credentials configured locally - [Configure AWS Credentials](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-set-up-credentials.html)
* Node.js - [Install Node.js 12](https://nodejs.org/en/), including the npm package management tool.

_Optional_
* Docker - for local testing [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community).

### Custom resources
AWS Lex isn't included as a resource in Cloudformation.

This project uses custom resources for [lex-bot](https://github.com/danblundell/cfn-lex-bot), [lex-intent](https://github.com/danblundell/cfn-lex-intent), and [lex-slot-type](https://github.com/danblundell/cfn-lex-slot-type)

To be able to deploy this Cloudformation stack, you'll need to have the custom resources deployed to your AWS Account in the same region first.

## Project structure
It includes the following files and folders:

- `src` - Code for the application's Lambda function(s).
- `events` - Invocation events that you can use to invoke the functions locally.
- `__tests__` - Unit tests. 
- `template.yml` - A template that defines the application's AWS resources for deployment.

Resources for this project are defined in the `template.yml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.

## Development
<tbw>

### Unit tests

Tests are defined in the `__tests__` folder in this project. Use `npm` to install the [Jest test framework](https://jestjs.io/) and run unit tests.

```bash
aws-sam-feedback-bot$ npm install
aws-sam-feedback-bot$ npm run test
```

## Deployment

The AWS SAM CLI is an extension of the AWS CLI that adds functionality for building and testing serverless applications.

To build and deploy the application for the first time, run the following in your shell:

```bash
sam build
sam deploy --guided
```

`sam build` will build the source of the application. 

`sam deploy --guided` will package and deploy the application to AWS, with a series of prompts.

> _Note_: if you use AWS profiles for your AWS credentials you'll need to add the `--profile <profile name>` flag to the `sam deploy` command

After running the deploy command, you'll get a series of prompts:

* **Stack Name**: The name of the stack to deploy to CloudFormation. This should be unique to your account and region, and a good starting point would be something matching your project name.
* **AWS Region**: The AWS region you want to deploy your app to.
* **Confirm changes before deploy**: If set to yes, any change sets will be shown to you before execution for manual review. If set to no, the AWS SAM CLI will automatically deploy application changes.
* **Allow SAM CLI IAM role creation**: Many AWS SAM templates, including this example, create AWS IAM roles required for the AWS Lambda function(s) included to access AWS services. By default, these are scoped down to minimum required permissions. To deploy an AWS CloudFormation stack which creates or modified IAM roles, the `CAPABILITY_IAM` value for `capabilities` must be provided. If permission isn't provided through this prompt, to deploy this example you must explicitly pass `--capabilities CAPABILITY_IAM` to the `sam deploy` command.
* **Save arguments to samconfig.toml**: If set to yes, your choices will be saved to a configuration file inside the project, so that in the future you can just re-run `sam deploy` without parameters to deploy changes to your application.

## Use the AWS SAM CLI to build and test locally

Build your application by using the `sam build` command.

```bash
aws-sam-feedback-bot$ sam build
```

The AWS SAM CLI installs dependencies that are defined in `package.json`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

## Local invocation

Test a single function by invoking it directly with a test event. An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

Run functions locally and invoke them with the `sam local invoke` command.

Example: 

```bash
aws-sam-feedback-bot$ sam local invoke feedbackBotValidation --event events/repeat_0.json
```

## Fetch, tail, and filter Lambda function logs

To simplify troubleshooting, the AWS SAM CLI has a command called `sam logs`. `sam logs` lets you fetch logs that are generated by your Lambda function from the command line. 

In addition to printing the logs on the terminal, this command has several nifty features to help you quickly find the bug.

**NOTE:** This command works for all Lambda functions, not just the ones you deploy using AWS SAM.

```bash
aws-sam-feedback-bot$ sam logs -n feedbackBotValidation --stack-name <STACK-NAME> --tail
```

**NOTE:** This uses the logical name of the function within the stack. This is the correct name to use when searching logs inside an AWS Lambda function within a CloudFormation stack, even if the deployed function name varies due to CloudFormation's unique resource name generation.

You can find more information and examples about filtering Lambda function logs in the [AWS SAM CLI documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-logging.html).

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
aws cloudformation delete-stack --stack-name Feedback-template
```

## Resources

For an introduction to the AWS SAM specification, the AWS SAM CLI, and serverless application concepts, see the [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html).

Next, you can use the AWS Serverless Application Repository to deploy ready-to-use apps that go beyond Hello World samples and learn how authors developed their applications. For more information, see the [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/) and the [AWS Serverless Application Repository Developer Guide](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/what-is-serverlessrepo.html).
