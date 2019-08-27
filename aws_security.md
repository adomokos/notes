## AWS Security

### Deployment Strategies for AWS

* Manual upload
* Source control
* Elastic Beanstalk
* CloudFormation
* S3

Elastic Beanstalk is great for one, single application.
You can use CloudFormation to create Beanstalk services.

#### AWS Shared Security Model

AWS is responsible for part of the security of your account.
You are responsible for the rest, with help from tools & services provided by AWS.

Three types of services in AWS
1. Infrastructure Services (EC2, VPC) - most control a user can have, AMZN provides Amazon Machine Images
2. Container Services (RDS, Beanstalk)
3. Abstract Services (DynamoDB, SQS)

### Deploying Applications to AWS

CloudFormation can reference other CloudFormation templates.
Can manage up-to 100 resources.

### Launch EC2 instance

This is a command to create EC2 instance:

```
aws ec2 run-instances --image-id ami-035b3c7efe6d061d5 \
  --count 1 --instance-type t2.micro --key-name "EC2 Keypair" \
  --security-group-ids sg-xyz --subnet-id subnet-xyz \
  --associate-public-ip-address \
  --profile=personal | jq
```

Learn about an EC2 instance:

```
aws ec2 describe-instance-status --instance-ids i-0ca54263cd4df44f3 \
  --profile=personal | jq
```

Terminate instance(s):

```
aws ec2 terminate-instances --instance-ids  i-0c73cbfcbbed2b7cb \
  --profile=personal | jq
```
