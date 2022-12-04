# Identity and Access Management (IAM)

## AWS Account Root User
- Created by default and has complete access to all AWS services and resources in the account
- Not to be used for everyday tasks (and instead, create an administrator user)
- Always secure root account with MFA 
- Certain tasks still require root credentials (for e.g., activating IAM access to the billing and cost management console) - [Click here for more information](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)

<br />

## AWS IAM Service
- Global service for centrally managing access to AWS resources 
- AWS resources are accessed via their respective APIs 

<br />

## Principals
- Entity that can request an action or operation on an AWS resource 
- E.g., IAM Users, IAM Roles, AWS Service, Identity Provider (IdP) or Federated User (external identities that are not managed directly by AWS IAM)

<br />

## IAM User
- Has no permissions by default 
- Has their own credentials 
- Always good practice to enable MFA and enforce strong password policies 

<br />

## Ways to Access AWS Services 
1. AWS Management Console Access (GUI): Create a password for the user 
2. Programmatic Access (AWS CLI or SDK): Create access key (access key ID and secret access key)
    - AWS provides SDK for Java, Python and .NET 
    - To set up AWS CL, use the `aws configure` command with the AWS Access Key, AWS Secret Access Key, Default Region Name and Default Output format (json, yaml, yaml-stream, text and table)

<br />

## IAM User Group
- Collection of IAM Users 
- Easier to manage permissions 
- Cannot nest groups 
- IAM Users can belong to multiple groups 

<br />

## IAM Role
- Temporary AWS Credentials 
- Can be delegated to IAM Users, IAM Groups or AWS Services 
- Permissions are only valid when assuming the role and any previously inherited permissions do not apply
- How to assume role? Assume a role via a trusted principal and make an API call to AWS STS for the temporary and limited-privilege credentials (access key ID, secret access key and a security token)
    - IAM User, IAM Group, AWS Service needs to have a policy attached that grants them permission to call `sts:AssumeRole` on the desired role (Federated Identities use `sts:AssumeRoleWithSAML` or `sts:AssumeRoleWithWebIdentity`)
    - Attach trust policy on the role 
    - [Click here for reference](https://stackoverflow.com/questions/34922920/how-can-i-allow-a-group-to-assume-a-role)
- Use Cases
    - Cross account access
    - **Temporary account access**
    - Apply least privilege 
    - Audit 
    - Access for AWS Services 
    - Access to Federated Identities 

<br />

## Security Policies
- Attached to an identity or resource and evaluated when a request is made 
- Types of Policies 
    1. Identity-based Policies: Managed or inline policies to IAM identities (users, groups and roles)
    2. Resource-based Policies: Inline policies to resources (e.g., S3 Bucket Polcies and IAM Role Trust Policies)
    3. AWS Organization Service Control Policies (SCPs): Maximum permissions for account members of an organization or organization unit (OU)
    4. IAM Permission Boundaries: Set maximum permissions that an IAM entity (user or role) can receive 

<br />

### **General Principle of Access Control: Always enforce Principle of Least Privilege**