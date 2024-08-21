# aws-ssm-send-command-await

Usage example:

```yaml
- name: Execute SSM Command
  uses: ordinov/aws-ssm-send-command-await@main
  with:
    aws-region: "eu-west-1"
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    instance-tag-name: "YOUR_INSTANCE_TAG_NAME"
    command: mkdir -p /tmp/test && echo "done"
```
