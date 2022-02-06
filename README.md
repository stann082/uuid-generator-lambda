# UUID Generator
### Installation
Source the dependencies
```bash
go get "github.com/aws/aws-lambda-go/lambda"
```
Build the binary
```bash
go build uuid.go
```
### Deploy
Create an IAM role for Lambda
```bash
aws iam create-role --role-name lambda-basic-execution --assume-role-policy-document file://lambda-trust-policy.json
```
Aattach permissions to the role
```bash
aws iam attach-role-policy --role-name lambda-basic-execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```
Check if the role was created (optional)
```bash
aws iam get-role --role-name lambda-basic-execution
```
Zip up the binary
```bash
zip uuid.zip uuid
```
Create AWS Lambda function
```bash
aws lambda create-function \
--function-name uuid-generator \
--zip-file fileb://uuid.zip \
--handler uuid \
--runtime go1.x \
--role "arn:aws:iam::<ACCOUNT_ID>:role/lambda-basic-execution"
```
Invoke the function (optional)
```bash
aws lambda invoke \
--function-name uuid-generator \
--invocation-type "RequestResponse" \
response.txt
```

