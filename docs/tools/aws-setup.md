# :fontawesome-brands-aws: AWS Setup

## 1. Install AWS CLI

:simple-homebrew: install [awscli](https://formulae.brew.sh/formula/awscli)

```
brew install awscli
```

## 2. IAM Identity Center
1. Enable IAM identity center management
2. Create an user
3. Create a group (for SuperRoots)
4. Create a Permission Set
5. Create AWS accounts
6. Do the below

```{ ..console}
$ aws configure sso
SSO session name (Recommended): VietThan-Admin-SSO
SSO start URL [None]: https://d-9067c6f14b.awsapps.com/start
SSO region [None]: us-east-1
SSO registration scopes [sso:account:access]:
Attempting to automatically open the SSO authorization page in your default browser.
If the browser does not open or you wish to use a different device to authorize this request, open the following URL:

https://oidc.us-east-1.amazonaws.com/authorize........
The only AWS account available to you is: <aws account>
Using the account ID <aws account>
The only role available to you is: AdministratorAccess
Using the role name "AdministratorAccess"
CLI default client Region [None]: us-east-1
CLI default output format [None]:
CLI profile name [AdministratorAccess-<aws account>]: VietThan-Admin-SSO

To use this profile, specify the profile name using --profile, as shown:

aws s3 ls --profile VietThan-Admin-SSO
```

## 3. Login

``` { ..console }
# 1. login with AWS CLI
aws sso login --profile VietThan-Admin-SSO

# export the environment variables of the account
eval "$(aws configure export-credentials --profile VietThan-Admin-SSO --format env)"

# export region
export AWS_DEFAULT_REGION="us-east-1"
```