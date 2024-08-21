# AWS SSM Send-Command

This action helps you to execute remote bash command for AWS EC2 instance **without SSH or other accessing**.

(This action internally uses AWS SSM Send-Command.)

## Contents

- [Requirements](#Requirements)
- [Usage example](#Usage-example)
- [Inputs](#Inputs)
- [Error Handling](#Error-Handling)

## Requirements

1. To use this action, you have to set AWS IAM Role `AmazonSSMFullAccess` to your IAM user.
2. Also your EC2 Instance must have IAM Role including `AmazonSSMFullAccess`.

## Usage example

```yml
name: AWS SSM Send-Command Example

on:
  push:
    branches: [master]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Execute SSM Command
        id: ssm_command
        uses: your-org/your-repo/.github/actions/execute-ssm-command@v1
        with:
          aws-region: 'eu-west-1'
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          instance-tag-name: 'YOUR_INSTANCE_TAG_NAME' # identify your EC2 using instance tag name..
          instance-id: 'i-0123456789abcdef0'  # .. OR directly the instance ID
          command: |
            echo "This is a test with \"quotes\" and 'single quotes'" && \
            echo "Another line with \$VARIABLE"

      - name: Print Command Output
        run: |
          echo "Command Output: ${{ steps.ssm_command.outputs.command-output }}"
          echo "Command Error: ${{ steps.ssm_command.outputs.command-error }}"
```

## Inputs

### `aws-access-key-id`

**Required** Your IAM access key id.

### `aws-secret-access-key`

**Required** Your IAM secret access key id.

### `aws-region`

**Required** AWS EC2 Instance region. (e.g. us-west-1, us-northeast-1, ...)

### `instance-id`

**Required if instance-tag-name is not defined** The id of AWS EC2 instance id (e.g i-xxx...)

### `instance-tag-name`

**Required if instance-id is not defined** The tag Name of AWS EC2 instance id (e.g "MY PRETTY NICE EC2")

### `command`

Bash command you want to execute in a EC2 instance.

### `comment`

Logging message attached AWS SSM.

```yml
# default
comment: Executed by Github Actions
```

## Error Handling

### AccessDeniedException

This error occurs when you are not set AWS IAM role about SSM. Please set the IAM permission `AmazonSSMFullAccess` (recommended)

### InvalidInstanceId: null

This error occurs when you are not attach AWS IAM role to your EC2 instance. Please set the IAM role `AmazonSSMFullAccess` (recommended)

> In almost error cases, those issues would be resolved when you set IAM Role to your `AWS Account` and `EC2 IAM Role`.