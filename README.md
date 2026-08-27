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

# Get the remote command's exit code (opt-in)
./sssh --command 'exit 7' --exit-code ; echo $?  # => 7

# Run port forwarding
./sssh --remote-host rds.example.com --remote-port 3306 --local-port 13306

# Specify OTP for MFA authentication
./sssh --profile foo-profile --otp 123456

# Help
./sssh --help
```
**Note**: Before running the script, ensure that the AWS CLI profile you use is configured to output in JSON format. Otherwise, the script will crash. Running `aws configure` only sets the `default` profile, so configure the profile you pass to `--profile` explicitly: `aws configure set output json --profile foo-profile`.

**Note**: By default, the exit code of the remote command is NOT reflected in the exit code of this script (an ECS Exec limitation). Use `--exit-code` to opt in; the command is then wrapped in `/bin/sh -c` and the exit code is relayed, which makes it usable from CI pipelines and AI agents.

**Note**: Since v5.0.0, all informational messages (date, Profile/Cluster/Service/Task/Container, the executed command line) go to stderr. stdout carries only the output of the remote command, so `./sssh --command 'php -v' | grep PHP` works as expected.

**Note**: Do not embed passwords or tokens in `--command`. The command line is shown on screen, kept in your local shell history, and recorded in CloudTrail; with ECS Exec logging enabled it may also be stored in CloudWatch Logs / S3.

## Special thanks to contributor
- [leewc](https://github.com/leewc)
- [bamkuhen](https://github.com/bamkuhen)
- [florianfelsing](https://github.com/florianfelsing)
