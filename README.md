# Centralized Security Management Setup in AWS Using Security Hub & Inspector

- Prerequisites
  - Root Account Access: Access to the root account for account creation and configuration.

  - AWS Organization: Set up an AWS Organization to manage multiple accounts.

  - IAM Permissions: Ensure the root user has the necessary permissions to manage roles and services
 


## Step-by-Step Process

  - 1. Sign in to the Root Account
  - Log in to the AWS Management Console using your Root Account credentials.

  - 2. Create the Security Account
  - Navigate to AWS Organizations:

  - Go to the AWS Organizations service in the AWS Management Console.



  - Account Name: Name this account something like Security (this will be used for centralized security management).

  - Email Address: Provide the email address.

  - AWS will automatically create an IAM role named OrganizationAccountAccessRole for this account, allowing it to interact with AWS Organizations.
  - 3. Create the Demo Account (Security1):

  - Similarly, create another account for Security1, which will be the demo account for security management.
