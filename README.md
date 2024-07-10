# cloud_uploader

Upload files to your AWS S3 instance via the cli by leveraging the AWS CLI.  

## Table of contents

[Overview]
[Getting started]
[Preprequisites/Requirements]
[Usage]
[Features]
[Resources]

## About this project

As part of my [Learn to Cloud](https://learntocloud.guide/phase1/) journey, I wrote this script that lets anyone upload files to their AWS S3 instance via the cli, leveraging the AWS CLI V2. It helped me strengthen my Linux and bash scripting skills.

### Overview

This script is written to be run on an Ubuntu linux machine, so it may run into compatibility issues if run on a different OS.

### Getting started

The following resources and packages are required to use this script:

### Prerequisites/Requirements

* aws account
  * AWS S3 instance
  * IAM User
  * IAM Role
* python (for aws cli)
* aws cli
* pv

#### Configuring AWS CLI

(If there's a part of this section, or a concept, you don't understand, check the resources section of this readme doc. I've linked a few resources that you'll find helpful)

For the AWS CLI to access your AWS resources, it needs to be configured with the necessary access credentials. I've chosen the 'config file' option from the available options because it offers some extra security (e.g. access restriction, encryption) and is well-suited for people who typically have several uploads to do.

– The AWS CLI can read all access configurations from the '~/.aws/config' file, so that's where you'll be keeping your access credentials. If it doesn't already exist, create it
– Generate an access key for the IAM User profile you'll be using for this operation (NEVER USE YOUR ROOT USER ACCOUNT, CREATE A NEW USER IN THE IAM)
– Add the access credentials to the config file you created earlier using the format below:

```html
[default]
aws_access_key_id = your access key id
aws_secret_access_key = your secret access key
region = your aws account region
output = your options are 'json' or 'text' (without the quotes)

````

Now, the AWS CLI can access the IAM User profile in your account, but it still can't upload to your aWS S3 instance.

If there isn't already one, create an IAM role and assign it the AwsS3FullAccess policy. Then add the IAM User above to the role as a trusted entity. Now we're one step away from allowing the IAM User 'assume' this role and inherit the AwsS3FullAccess permissions.

From the IAM User profile, create a permission policy that allows it to assume the IAM role.

For the final part of our AWS CLI configuration, you'll add a profile in the config file which the AWS CLI will then use to perform the upload operations. To do this, add the following to the config file:

```html
[profile 'enter a profile name here']
role_arn= your IAM role's ARN
source_profile = default

```

### Usage

```bash

bash /path/to/this/script path/to/file name-of-target-bucket config-file-profile-to-use [optional new-filename]

#Example command

bash ./aws_upload ./datafile c1-assets awss3-uploader datafilefrommymachine

```

### Features

– [x] Server-side encryption using your keys (key filed saved in source file dir)
– [x] Storage class selection – interactive
– [x] File sync - overwrite or rename dupliate files
– [x] Upload progress bar

### Resources

– [Setting up the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html)
– [AWS CLI Configuration and Credential files setting](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
– [Authenticate with IAM User credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html)
– [Using an IAM role in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html)