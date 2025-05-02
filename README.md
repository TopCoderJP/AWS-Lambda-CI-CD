GitHub Actions CI/CD for Lambda Functions and CloudFormation Validation
=======================================================================


This project demonstrates a serverless CI/CD pipeline using GitHub Actions to automate AWS Lambda deployments and validate CloudFormation templates.

1.  Automatically Deploying Lambda Functions
2.  Automating CloudFormation Validation for Pull Requests

Both workflows are designed to enhance DevOps practices by ensuring that infrastructure changes are consistently deployed and validated before merging.

Workflow Architecture
---------------------



1.  Lambda Deployment Pipeline:

    -   Triggers on pushes to main branch when lambda code changes
    -   Packages code into zip file
    -   Updates existing Lambda function via AWS CLI
2.  CloudFormation Validation Pipeline:

    -   Triggers on PR events involving CloudFormation templates
    -   Creates temporary test stacks
    -   Comments on PR with results
    -   Automatically cleans up test resources

Business Value
--------------


-   Reduces deployment errors with automated validation
-   Speeds up development cycle with automatic deployments
-   Improves code quality through infrastructure testing before merge
-   Eliminates manual AWS console work

* * * * *



* * * * *

About The Project
-----------------


This repository features two distinct GitHub Actions workflows:

### Part 1: Automatically Deploying Lambda Functions



This workflow triggers on push events to the main branch. When changes are detected in the `lambda/` directory, it automatically deploys the updated Lambda function to AWS. This ensures that any code changes to the Lambda function are consistently deployed without manual intervention.

Key features:

-   Automated deployments on code push to the main branch.
-   AWS credentials management via GitHub Secrets.
-   Error handling to prevent further steps if deployment fails.

### Part 2: Automating CloudFormation Validation


This workflow triggers on pull request events and handles the following tasks:

-   Validates CloudFormation templates when a PR is opened or updated.
-   Deploys a test CloudFormation stack to AWS for validation.
-   Comments on the pull request with deployment status and details.
-   Cleans up (deletes) the test stack when the PR is merged.

Key features:

-   Ensures infrastructure changes are validated before merging.
-   Automatically removes test stacks after merging.
-   Supports multiple PR event types, including `opened`, `updated`, `reopened`, and `closed`.



* * * * *

Built With
----------



This project uses the following tools and frameworks:

-   [![GitHub Actions](https://camo.githubusercontent.com/dcd09765699bd4656f9b882d18ed49a1b74b888dd8a9c8dff6b15a9e4d2da9ff/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4769744875625f416374696f6e732d3230383846463f7374796c653d666f722d7468652d6261646765266c6f676f3d6769746875622d616374696f6e73266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/dcd09765699bd4656f9b882d18ed49a1b74b888dd8a9c8dff6b15a9e4d2da9ff/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4769744875625f416374696f6e732d3230383846463f7374796c653d666f722d7468652d6261646765266c6f676f3d6769746875622d616374696f6e73266c6f676f436f6c6f723d7768697465)
-   [![AWS Lambda](https://camo.githubusercontent.com/6a8a6ed224074458a2727fca7ca3cbbf3ffbb14f6244e887b81527747001f831/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4157535f4c616d6264612d4630393030303f7374796c653d666f722d7468652d6261646765266c6f676f3d616d617a6f6e2d617773266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/6a8a6ed224074458a2727fca7ca3cbbf3ffbb14f6244e887b81527747001f831/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4157535f4c616d6264612d4630393030303f7374796c653d666f722d7468652d6261646765266c6f676f3d616d617a6f6e2d617773266c6f676f436f6c6f723d7768697465)
-   [![AWS CloudFormation](https://camo.githubusercontent.com/3a6cf33870a8a746507c079fb0b48f2965d589fdf97ebb1132af67f3d82cf749/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4157535f436c6f7564466f726d6174696f6e2d4630393030303f7374796c653d666f722d7468652d6261646765266c6f676f3d616d617a6f6e2d617773266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/3a6cf33870a8a746507c079fb0b48f2965d589fdf97ebb1132af67f3d82cf749/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d4157535f436c6f7564466f726d6174696f6e2d4630393030303f7374796c653d666f722d7468652d6261646765266c6f676f3d616d617a6f6e2d617773266c6f676f436f6c6f723d7768697465)

* * * * *

Getting Started
---------------


### Prerequisites


You need the following tools installed:

1.  AWS CLI Install the AWS Command Line Interface by following the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2.  GitHub CLI (optional) Install GitHub CLI if you want to manage PRs from the command line:

    ```source-shell
    brew install gh
    ```

3.  Basic Git and GitHub Knowledge Ensure you have a basic understanding of Git version control and GitHub repositories.

* * * * *

### Installation


1.  Clone the Repository:

    ```source-shell
    git clone git@github.com:TopCoderJP/lambda-cicd.git
    cd lambda-cicd
    ```

2.  Set Up AWS Credentials: Use the AWS CLI to configure your credentials locally:

    ```source-shell
    aws configure
    ```

3.  Push to GitHub and Create a PR: Push your changes to a branch and open a pull request:

    ```source-shell
    git checkout -b feature/add-new-cloudformation-template
    git push origin feature/add-new-cloudformation-template
    ```

4.  Review Workflow Execution: Go to the Actions tab in your GitHub repository to monitor workflow execution.


* * * * *

Usage
-----


### Part 1: Automatically Deploying Lambda Functions



1.  Set Up AWS Credentials Add AWS credentials as GitHub Secrets to securely authenticate deployments:

    -   `AWS_ACCESS_KEY_ID`
    -   `AWS_SECRET_ACCESS_KEY`
2.  Create a Push Workflow Add the following workflow file in `.github/workflows/lambda.yml`:

    ```source-yaml
    name: Deploy AWS Lambda

    on:
      push:
        branches:
          - main
        paths:
          - 'lambda/**'

    jobs:
      deploy-lambda:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2

          - name: Set Up Python
            uses: actions/setup-python@v2
            with:
              python-version: '3.11'

          - name: Install Dependencies
            run: |
              python -m pip install --upgrade pip
              pip install -r lambda/requirements.txt -t lambda/

          - name: Configure AWS Creds
            uses: aws-actions/configure-aws-credentials@v2
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: ap-northeast-1

          - name: Deploy Lambda Function
            run: |
              cd lambda/
              zip -r lambda.zip .
              aws lambda update-function-code --function-name my-test-cicd-lambda --zip-file fileb://lambda.zip
    ```

3.  Trigger the Workflow Push changes to the `lambda/` directory on the `main` branch. The workflow will automatically deploy the updated Lambda function.

4.  Verify the Deployment Check the AWS Lambda Console to verify that the function was updated.

* * * * *

### Part 2: Automating CloudFormation Validation


1.  Create a CloudFormation Template Add the following CloudFormation template in `cloudformation/s3-bucket.yml`:

    ```source-yaml
    AWSTemplateFormatVersion: '2010-09-09'
    Description: 'S3 Bucket for our CICD PR'

    Parameters:
      Environment:
        Type: String
        Default: test
        AllowedValues:
          - test
          - staging
          - production

    Resources:
      MyS3Bucket:
        Type: AWS::S3::Bucket
        Properties:
          BucketName: !Sub '${AWS::StackName}-${Environment}-bucket-v2'
          Tags:
            - Key: Environment
              Value: !Ref Environment
            - Key: Environment
              Value: GithubActions-CFN-Validation-Logic
    ```

2.  Create a PR Workflow Add the following workflow file in `.github/workflows/cfn-validate-pr.yml`:

    ```source-yaml
    name: Validate CloudFormation on PR

    on:
      pull_request:
        paths:
          - 'cloudformation/**'

    jobs:
      validate-cfn:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2

          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: ap-northeast-1

          - name: Validate CloudFormation template
            run: |
              aws cloudformation validate-template --template-body file://cloudformation/s3-bucket.yml
    ```

* * * * *

Contributing
------------



Contributions are welcome! Follow these steps to contribute:

1.  Fork the repository
2.  Create a feature branch (`git checkout -b feature/new-feature`)
3.  Commit your changes (`git commit -m 'Add new feature'`)
4.  Push to your branch (`git push origin feature/new-feature`)
5.  Open a pull request
