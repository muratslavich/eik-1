# AWS cli

[docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

```
$ docker run --rm -it amazon/aws-cli command
```

\--rm  - to clean up your files

\-it      - to enable pseudo-TTY

shorten docker command

```
$ alias aws='docker run --rm -it amazon/aws-cli'
$ alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
$ was --version
```

Share host files, credentials, environment variables, and configuration

```
$ docker run --rm -it -v ~/.aws:/root/.aws amazon/aws-cli command
```

providing credentials and configuration

```
$ docker run --rm -it -v ~/.aws:/root/.aws amazon/aws-cli s3 ls
```

Downloading an Amazon S3 file to your host system

```
$ docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli s3 cp s3://aws-cli-docker-demo/hello .
```

. - to {pwd}

Use env variables in the command

```
$ docker run --rm -it -v $env:userprofile\.aws:/root/.aws -e AWS_PROFILE amazon/aws-cli s3 ls

-e AWS_PROFILE
```

### Configuration

Set up aws cli :

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

Output format:

* json
* yaml
* yaml-stream
* text
* table

AWS CLI can use profiles.

By default, the AWS CLI uses the default profile.

```
$ aws configure --profile produser
$ aws s3 ls --profile produser
```

To update these settings, run AWS configure again (with or without the --profile parameter, depending on which profile you want to update) and enter new values as appropriate.

### Configuration settings

* [command line options](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-options.html)
* [environment variables](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
* credential/configure files in \~/.aws
* [container cred](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)
* [Instance profile credentials](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)

### Using examples

[docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-services.html)

```
$ aws configure list
$ aws configure list-profiles
$ aws configure get <property_name>
$ aws configure set <property_name> <property_value> --profile <profile_name>
```

[ec2 describe-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html)

```
$ aws ec2 describe-instances
[--filters <value>]
[--instance-ids <value>]
[--dry-run | --no-dry-run]
[--cli-input-json <value>]
[--starting-token <value>]
[--page-size <value>]
[--max-items <value>]
[--generate-cli-skeleton <value>]
```

key-pair

```
$ aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem

$ aws ec2 describe-key-pairs [--key-name MyKeyPair]

$ aws ec2 delete-key-pair --key-name MyKeyPair
```

security-group

```
vpc
$ aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id vpc-1a2b3c4d

$ aws ec2 describe-security-groups --group-ids sg-903004f8

ec2-classic
$ aws ec2 create-security-group --group-name my-sg --description "My security group"

$ aws ec2 describe-security-groups --group-names my-sg

add rule
$ aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 22 --cidr x.x.x.x

delete security group
$ aws ec2 delete-security-group --group-name my-sg
```

Find your IP

```
curl https://checkip.amazonaws.com
```

instances

```
launch
$ aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids sg-903004f8 --subnet-id subnet-6e7f829

list
$ aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query "Reservations[].Instances[].InstanceId"

terminate
$ aws ec2 terminate-instances --instance-ids i-5203422c
```
