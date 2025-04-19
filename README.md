# Centralized Security Management Setup in AWS Using Security Hub & Inspector

- Prerequisites
  - Root Account Access: Access to the root account for account creation and configuration.

  - AWS Organization: Set up an AWS Organization to manage multiple accounts.

  - IAM Permissions: Ensure the root user has the necessary permissions to manage roles and services
 


## Step-by-Step Process
### Step 1
1. Sign in to the Root Account
  - Log in to the AWS Management Console using your Root Account credentials.

2. Create the Demo Account (security)
  - Navigate to AWS Organizations:

  - Go to the AWS Organizations service in the AWS Management Console.



  - Account Name: Name this account something like **Security** (this will be used for centralized security management).

  - Email Address: Provide the email address.

  - AWS will automatically create an IAM role named OrganizationAccountAccessRole for this account, allowing it to interact with AWS Organizations.

3. Create the Demo Account (Security1):

  - Similarly, create another account for **Security1**, which will be the demo account for security management.

### Step 2
1. Enable Service Access for Security Hub
Now, from your Root Account  enable service access for Security Hub across your AWS Organization:

```bash
aws organizations enable-aws-service-access --service-principal securityhub.amazonaws.com
```
  - Explanation:
    - The above command enables the service access for Security Hub across your entire AWS Organization. This ensures that Security Hub can manage and monitor security configurations for all accounts within the organization, including the Security Account and Security1 Account.

    - Where to run the command: Run this from the Root Account

### Step 3
1. Register Security Account as a Delegated Administrator for Security Hub

  - Run the following command to register your Security Account as a delegated administrator for Security Hub:

```bash
aws organizations register-delegated-administrator --account-id <SECURITY_ACCOUNT_ID of account named security > --service-principal securityhub.amazonaws.com
```
  - Explanation:
    - Above command provides administrative permissions to the Security Account for managing Security Hub in your AWS Organization. The Security Account will now be able to manage and monitor security settings and findings for all other accounts in your organization, such as the Security1 Account.

### Step 4
1. Enable Amazon Inspector Delegated Admin for Security Account
   
  - Enable the Security Account as the delegated administrator for Amazon Inspector:

```bash
aws inspector2 enable-delegated-admin-account --delegated-admin-account-id <SECURITY_ACCOUNT_ID of account named security>
```
  - Explanation:
    - Through above command Amazon Inspector of  security account will  be able to perform vulnerability assessments for other aws accounts.

### step 5
1. Enable Service Access for Account Service
  - To allow Security Account to manage and access resources in other accounts, run below command:

```bash
aws organizations enable-aws-service-access --service-principal account.amazonaws.com
```
  - Explanation:
    - This allows services like AWS Organizations to manage and perform tasks related to your accounts

### Step 6
1. Switch to Security Account

  - From CLI use access key and secret key of security account and login to security account.


### Step 7
1.  Enable Security Hub in the Security Account
  - Enable Security Hub in the Security Account to aggregate security findings from the Security1 Account:

```bash
aws securityhub enable-security-hub --region eu-north-1
```
  - explanation:
      - above command enables Security Hub in the Security Account to act as the central hub for managing security findings and configurations across the organization.
   

### step 8
1. Enable Amazon Inspector in the Security Account
  - Enable Amazon Inspector to monitor EC2, ECR, and Lambda resources:

```bash
aws inspector2 enable --resource-types EC2 ECR LAMBDA
```
Explanation:
This enables Amazon Inspector in the Security Account for vulnerability scanning of EC2 instances, ECR repositories, and Lambda functions.

### step 9
1. Add Security1 Account to Security Hub
  - Navigate to Security Hub Console in the **Security Account**:

  - In the Security Account, go to the Security Hub Console.

Add Security1 Account:

  - Under Configuration, click on Add Accounts.

  - Provide the Account ID of Security1 to invite it to Security Hub.


### step 10
1. Accept Security Hub Invitation in Security1 Account
  - Switch to the Security1 Account and enable security hub manually through portal.

  - In the Security Hub Console, accept the invitation to join Security Hub.

  - After accepting the invitation, refresh, and the status should change to Active 1/1.


### Step 11
1. Enable Amazon Inspector in the Security1 Account
  - Enable Amazon Inspector in Security1 Account for vulnerability assessments:

```bash
aws inspector2 enable --resource-types EC2 ECR LAMBDA
```
  - Explanation:
    - This command enables Amazon Inspector in the Security1 Account for vulnerability assessments on EC2 instances, ECR repositories, and Lambda functions.

### step 12
Verify Findings in Security Account
  - In the Security Account, retrieve aggregated findings from Security Hub:

```bash
aws securityhub get-findings --region eu-north-1
```
  - Explanation:
    - This command retrieves security findings from Security Hub for all accounts, including Security1, aggregated in the Security Account.


