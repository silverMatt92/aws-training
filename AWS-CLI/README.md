# AWS CLI Setup for Picnic Account Corp IT
In order to use the AWS CLI, you need to have your credentials and then follow
the official documentation to install and configure it.  
Credentials have been shared with you.

1. Install the CLI  
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html  

2. Configure the CLI:  
We're going to use a profile to keep credentials separated, depending on which
account you need to access.  
In order to do so, we're going to modify a couple of files in your home
directory.
The files to be modified are:  
```bash
./aws/credentials (Linux & Mac) or %USERPROFILE%\.aws\credentials (Windows)
```
where you need to configure something similar to this, replacing your personal
credentials:  
```bash
[picnic-corpit]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

and the configuration file:
```bash
~/.aws/config (Linux & Mac) or %USERPROFILE%\.aws\config (Windows)
```

where you may configure the profile to match a particular region and output
format:
```bash
[profile picnic-corpit]
region = us-east-1
output = json
```

In order to confirm that the AWS CLI is working, you should be able to check
the default VPC deployed in us-east-1, with the following command:
```bash
aws ec2 describe-vpcs --profile picnic-corpit
```

Please, remember that for every AWS CLI command you run, you need to specify
the profile option with --profile <profile-name> as the command above.
