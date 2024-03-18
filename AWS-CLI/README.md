# AWS CLI Setup with SSO
In order to use the AWS CLI with Picnic SSO, you need to first install the AWS CLI following
the official documentation, a summary is provided here:

1. Install the CLI  
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html  

2. Configure the CLI:  
We're going to use a profile to keep environments separated, depending on which
account you need to access.  
In order to do so, we're going to create/modify a file in your home
directory.
The file is:  
```bash
./aws/config (Linux & Mac) or %USERPROFILE%\.aws\config (Windows)
```
where you need to configure something like this:
```bash
[picnic-corpit]
sso_start_url = https://teampicnic.awsapps.com/start#
sso_region = eu-west-1
sso_account_id = 876319641360
sso_role_name = Administrator
region = eu-west-1
output = json
```

In order to confirm that the AWS CLI is working, you should be able to check
the default VPC deployed in us-east-1, with the following command:
```bash
aws ec2 describe-vpcs --profile picnic-corpit
```

Please, remember that for every AWS CLI command you run, you need to specify
the profile option with --profile <profile-name> as the command above.  
  
Please note that the SSO session needs to be refreshed after your session token expires, indeed if that happens you will 
encounter this output on your terminal:
```
The SSO session associated with this profile has expired or is otherwise invalid. To refresh this SSO session run aws sso login with the corresponding profile.
```
You can refresh your token by running the command:
```
aws sso login --profile picnic-corpit
```
This will redirect to your browser where you will need to re-authenticate via SSO
