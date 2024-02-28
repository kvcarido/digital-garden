---
title: CloudUploader CLI
draft: 
tags:
  - sapling
---
[Github link](https://github.com/kvcarido/clouduploader-cli)

This project originates from the [Learn to Cloud Guide](https://learntocloud.guide), a free and thoughtfully built outline of skills suited for those who want to dive into Cloud Computing. I found the overall style and approach to learning quite refreshing, as it prioritizes active learning through building and documentation. Riding off the momentum of the new year, combined with the instinctual itch to make the next big step in my IT career happen, I'm here to prove to myself (and I guess the internet) that I'm a woman of action. Let's dive into it.
## Project Overview
The goal was to create a [[shell]] script that uploads a file to cloud storage from the terminal. Having experience from my current role in scripting and AWS, I thought this was a practical way to bridge technologies I'm familiar with and document my approach for the sake of building.
### Setting up AWS
Before building the script, I needed to set up the appropriate [[AWS]] services for the script to function. For this project, we'll be using the AWS resources below:

| Resource | Description                                                                                                                |
| :------- | -------------------------------------------------------------------------------------------------------------------------- |
| IAM      | Identity and Access Management – helps securely control access to AWS resources by managing users, groups, and permissions |
| S3       | Simple Storage Solution – scalable storage that let's you store and retrieve data from the web                             |

In the AWS Console I created a new user and user group, which makes configuring proper permissions easier to manage. In the user group I attached the `AmazonS3FullAccess` policy* so the user can see and interact with the S3 buckets. Once created, navigate to the user's summary dashboard and generate an access key. After going through the prompts save the generated access and secret keys, which will be used to authenticate through the AWS CLI in the script.

> [!warning]
> *Giving full access is not advised in a production setting, but for the sake of time and learning, we'll go with it.

Instead of interacting with AWS Resources through the Console in the web browser, [AWS CLI](https://aws.amazon.com/cli/) enables us to interact with AWS programmatically through the terminal. For this project, we'll be utilizing the following commands:

| Command                 | Action                                                            |
| :---------------------- | ----------------------------------------------------------------- |
| `aws configure`         | Creates a `config` and `credentials` file located in `~/.aws`     |
| `aws s3 ls`             | Lists all buckets                                                 |
| `aws s3api head-bucket` | Used to determine if a bucket exists and permissions to access it |

### `clouduploader.sh`
After briefly reading through the documentation and [command reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html) for AWS CLI, getting set up and verified with working credentials didn't take much effort, and we were ready to build our script. Initially, I was super excited about tackling this project, so I started coding head first, equipped with more enthusiasm to build than a plan. The first attempt was entirely over-engineered, so I started from scratch and stepped back to approach this more methodically.

To really drill in coding fundamentals and best practices, I wanted to include as much error handling as possible and prioritize the user experience when interacting with the script by providing output to the terminal. 

The first loop checks if the AWS CLI is installed – if it is, the next loop checks if the directory `$HOME/.aws/config` contains files (`config` and `credentials`) within that directory, which indicates an AWS CLI profile has been configured. If any of these two loops are evaluated as false, it will prompt the user to install AWS CLI or configure an AWS CLI profile.

When setting up a profile for the first time, you'll need to enter your Access and Secret access keys from earlier.

![[aws-config-profile.png]]
Once an AWS CLI profile is configured on your machine, you'll have full access to the suite of commands to interact with AWS resources. Even though you'll be able to use all the commands available, you'll need proper permissions configured to the user profile to execute those commands successfully.

Once the configuration checks have passed, the script will present the user with the available S3 buckets for upload. It then prompts the user to type the bucket name to which they'd like to upload the file. 

Keeping in line with error handling, the script ensures all information submitted is valid. Using the `aws s3api head-bucket` command checks to see if the bucket exists and if the user has proper permissions to access it, but ultimately I wanted to make sure there were no spelling errors when asking for user input. One of the project's criteria is to confirm that the file to be uploaded exists, which was also utilized with a conditional loop.

If all checks are successful, the file path passed with the script execution is uploaded to the specified S3 bucket.

```bash
# Uploads file to selected bucket
BUCKET_PATH="s3://$BUCKET"
aws s3 cp "$1" "$BUCKET_PATH"
```

The full AWS file path must be passed when uploading files to a bucket, so `BUCKET_PATH` appends the user input to the required text. The `$1` variable represents the first argument passed with the shell script when executed. Command-line arguments (aka positional parameters) are built-in and make scripting more dynamic.

If all checks are successful again, the user will receive validation in the terminal. Using `aws s3 cp` copies a local file to a local in S3. `upload: ./test.txt to s3://aokpublic/test.txt` is the default output when the command runs successfully.
![[successful-upload.png]]
### Learnings and Parking Lot
> *This section is like a scratch pad for topics and concepts that need more pondering and editing. Think of it as the open browser tabs you'll eventually get back to.*

- [Configure AWS CLI to use IAM Identity Center (Authentication)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) 
- [[Bash Standard Streams]]
#### Stretch Goals
- [ ] Utilize IAM Identity Center to authenticate instead of keys
- [ ] File synchronization – if the file exists in the cloud, prompt the user to overwrite, skip, or rename
- [ ] Optimize distribution – better way to do this? Document adding to user's `$PATH` in `README`

