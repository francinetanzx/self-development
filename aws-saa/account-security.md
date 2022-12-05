# Identity and Access Management (IAM)

## AWS Account Root User
- Created by default and has complete access to all AWS services and resources in the account
- Not to be used for everyday tasks (instead, create an administrator user)
- Always secure root account with MFA 
- Certain tasks still require root credentials (for e.g., activating IAM access to the billing and cost management console) - [Click here for more information](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)

<br />

## AWS IAM Service
- Global service for centrally managing access to AWS resources 
- AWS resources are accessed via their respective APIs 

<br />

## Principals
- Entity that can request an action or operation on an AWS resource 
- IAM Users, IAM Roles, AWS Service, Identity Provider (IdP) or Federated User (external identities that are not managed directly by AWS IAM)

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
- Temporary AWS Credentials for Principals
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
        - Managed Policies - Can be attached (or reused) across multiple users, groups and roles 
            1. AWS-managed Policies: Created and managed by AWS (usually built for service access or job function)
            2. Customer-managed Policies (PREFERRED OVER INLINE POLICIES)
        - Inline Policies - Added to directly to a single user, group or role (1-to-1 relationship) and deleted alongside the identity  
    2. Resource-based Policies: Inline policies to resources (e.g., S3 Bucket Polcies and IAM Role Trust Policies)
    3. AWS Organization Service Control Policies (SCPs): Maximum permissions for account members of an organization or organization unit (OU)
    4. IAM Permission Boundaries: Set maximum permissions that an IAM entity (user or role) can receive 

<br />

## Identity-based Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FirstStatement",
      "Effect": "Allow",
      "Action": ["iam:ChangePassword"],
      "Resource": "*"
    },
    {
      "Sid": "SecondStatement",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ThirdStatement",
      "Effect": "Allow",
      "Action": [
        "s3:List*",
        "s3:Get*"
      ],
      "Resource": [
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ],
      "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}
    }
  ]
}
```
**Note: Sid and Condition are optional*

Precedence of Policy Statements:
1. Explicit deny
2. Explicit allow 
3. Implicit deny

<br />

## Resource-based Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AccountBAccess",
      "Effect": "Allow",
      "Principal": {"AWS": "444455556666"},
      "Action": ["s3:PutObject"],
      "Resource": [
        "arn:aws:s3:::DOC-EXAMPLE-BUCKET/folder123/*"
      ]
    }
  ]
}
```
**Note: Must list the principal account (user, role, federated user) and resource is optional*

**Practice defense in-depth by using both identity and resource based policies**

<br />

## Permission Boundaries 
An entity's permission boundary allows it to perform only the actions allowed by both its identity-based policies and its permission boundaries 

## AWS Organizations 
- Centralized management for all of your AWS accounts 
- Consolidate billing for all member accounts
- Create hierarchy by grouping accounts into Organizational Units (OU)
- Apply SCPs to control maximum permissions in every account under an OU 
    - *Note: Overlap between inherited SCPs and owned SCPs = Permissions that can actually be used
    - Only allow what is at the intersection of IAM permissions and SCPs
    - DO NOT GRANT PERMISSIONS, THEY FILTER 
    - Only available in an organization that has all features turned on (not just consolidated billing)
- Deploy standardized permissions across accounts 

### **General Principle of Access Control: Always enforce Principle of Least Privilege**