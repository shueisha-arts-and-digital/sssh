# UPDATE
- [ECS Exec is now available in the AWS Management Console
](https://aws.amazon.com/jp/about-aws/whats-new/2025/09/ecs-exec-aws-management-console/)

# sssh
## About
- Bash script to run ecs-exec on Amazon ECS Fargate containers.

## Prerequisites
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Session Manager plugin for the AWS CLI](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)
- [jq](https://stedolan.github.io/jq/download/)
- [peco](https://github.com/peco/peco#installation)

## Install
```bash
git clone https://github.com/shueisha-arts-and-digital/sssh.git
cd sssh
./sssh
```

## Usage
```bash
# Select profile, cluster, service, container, and task, and run sh
./sssh

# Run a one-off command on the container ("--opt value" and "--opt=value" both work)
./sssh --command "php -v"
./sssh --command="php -v"

# Pipes, redirects, and variable expansion require an explicit shell
./sssh --command 'sh -c "php -v | head -n 1"'

# Run port forwarding
./sssh --remote-host rds.example.com --remote-port 3306 --local-port 13306

# Specify OTP for MFA authentication
./sssh --profile foo-profile --otp 123456

# Help
./sssh --help
```
**Note**: Before running the script, ensure that your AWS CLI session profile is configured to output in JSON format. Otherwise, the script will crash. You can set the output format as JSON when you run `aws configure`.

**Note**: The exit code of the remote command is NOT reflected in the exit code of this script (an ECS Exec limitation), so it is not suitable for CI pipelines.

**Note**: Do not embed passwords or tokens in `--command`. The command line is shown on screen, kept in your local shell history, and recorded in CloudTrail; with ECS Exec logging enabled it may also be stored in CloudWatch Logs / S3.

## Special thanks to contributor
- [leewc](https://github.com/leewc)
- [bamkuhen](https://github.com/bamkuhen)
- [florianfelsing](https://github.com/florianfelsing)
