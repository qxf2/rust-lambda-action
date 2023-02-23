# rust-lambda-action
A GitHub Action to deploy AWS Lambda functions written in Rust. 

### Background
This uses https://github.com/awslabs/aws-lambda-rust-runtime, a Rust runtime for AWS Lambda. Cargo Lambda is a Cargo plugin, or subcommand that provides several commands to run, build and deploy Rust functions on Lambda.

### Use
Deploys the specified directory within the repo to the Lambda function. Uses Amazon Linux 2 runtime for building Lambda functions.

### Pre-requisites
In order for the Action to have access to the code, use the `actions/checkout@master` job before it.

### Structure
Lambda code should be structured normally as Lambda would expect it.

   
### Inputs
- `lambda_directory`
    The directory which has the Lambda code
- `iam_role`
    The AWS IAM role required to deploy this Lambda
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`   

### Implementation
1. Used cargo lambda to first build for Amazon Linux 2 runtime
2. Once the code is built, used cargo lambda to upload function to AWS. This step requires IAM role and AWS credentials.


### Example workflow
```yaml
name: deploy-dummy-lambda
on: 
  push:
    branches:
      - master
    paths:
      - 'dummy_lambda/**'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Deploy code to Lambda
      uses: sravantit25/rust-lambda-action@v1.0.3
      with:
        lambda_directory: 'dummy_lambda'
        iam_role: arn:aws:iam::285993504765:role/lambda-deployer
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_DEFAULT_REGION: ${{ secrets.SKYPE_SENDER_REGION }}

```
