# AWS Identity and Access Management (IAM)

Allows how people and programs are allowed to manipulate the infrastructure.
IAM uses concepts such users, groups and ACL policies to control the accounts, what services and resources can use and how can use them.

- IAM is not an identity store/authorization system for applications, the permissions are just to manipulate AWS infrastructure not to your application. Nevertheless your on-premise
Active Directory can be extended into the cloud to continue to fill that need.AWS Directory Service, which is an Active Directory-compatible directory service that can work on its own or integrate
with your on-premises Active Directory.
- You can manipulate IAM through the management console, AWS CLI and via AWS SDK

## Principals
Is an IAM identity allowed to interact with the resources. Can be permanent or temporary, and it can be represent a human or an application and exist 3 types of principals: root users, IAM users and roles/temporary security tokens.

### Root users
Is the first AWS account created once you been registered on AWS that has complete access to all AWS  services and resources in the account. this is called **root user**. Is strongly recommended not use this account for every tasks.

### IAM user
Are persistent identities to represent people or applications. Can be created by principals

### Roles/Temporary Security Tokens
Roles are used to grant specific privileges to specific actors for a set duration of time. These actors can be authenticated by AWS or some trusted external system. When one of these actors assumes a role, AWS provides the actor with a temporary security token from the AWS Security Token Service (STS) that the actor can use to access to the AWS services.

Roles and temporary security tokens enable a number of use cases:

 - EC2 role: granting permissions to applications running on EC2 instances
 - Cross-Account Access: Granting permissions to users from other AWS accounts.
 - Federation: Granting permissions to users authenticated by a trusted external system.

### EC2 Roles
Using IAM roles for Amazon EC2 removes the need to store AWS credentials in a configuration file.
Roles grant the option to attach the policy to the EC2 instances and the instance/application on the instance can access to the services like s3, dynamo etc with a temporary token and pasing it to the API that it's automatically.

### Cross-account Access
Grant access to AWS resources to IAM users in other AWS accounts. This is highly recommended as a best practice, as opposed to distributing access keys outside your organization.

### Federation
Similarly, web-based applications may want to leverage web-based identities such as Facebook, Google, or Login with Amazon. IAM Identity Providers provide the ability to federate these outside identities with IAM and assign privileges to those users authenticated outside of IAM.

Can integrated with 2 different types of outside Identity Providers (IdP) For federating web identities such as Facebook, Google, or Login with Amazon, IAM supports integration via OpenID Connect (OIDC).

For federating internal identities, such as Active Directory or LDAP, IAM supports integration via Security Assertion Markup Language 2.0 (SAML)

## Authentication
There are three ways that IAM authenticates a principal:

- **username/Password**: the the principal is a Human it's provided a username and password to verify their identity.
- **Access Key**: Is a combination of access ky ID and a access secret key. Used principally when you use your infrastructure via API .
- **Access key /Session Token**: When a process operates under an assumed role, the temporary security token provides an access key for authentication. In addition to the access key (remember that it consists of two parts), the token also includes a session token. Calls to AWS must include both the two-part access key and the session token to authenticate.

## Authorization

Is handled in IAM by defining specific privileges in policies and associating those policies with principals.

### Policies
Is a JSON document that fully defines a set of permissions to access and manipulate AWS resources. each
permission defining:

 - **effect**: Allow or deny
 - **service**: For what service does this permission apply?
 - **resource**: Specific AWS infrastructure for which this permission applies. and is provided by an Amazon Resource Name (ARN)
 - **action**: The subset of actions within a service that the permission allows or denies.
 - **condition**: Defines one or more additional restrictions that limit the actions allowed by the permission

## Associating Policies with Principals
There are several ways to associate a policy with an IAM user; we are going to cover just the most common.
A policy can be associated directly with an IAM user in one of two ways:

 - **user policy**: These policies exist only in the context of the user to which they are attached. In the console, a user policy is entered into the user interface on the IAM user page.
 - **managed policies**:
 - **group policy**
 - **managed policies**

 ![iam associate policies](Figures\chapter6-figure10-Associating-policies-with-principals.PNG)


## Other key features
 Principals, authentication, and authorization, there are several other features of the IAM service


### Multi-Factor authentication (MFA)
Can add an extra layer of security to your infrastructure by adding a second method of authentication. Can be assigned to any IAM user account, whether the account represents a person or application

### Rotating keys
Is a security best practice to rotate access keys associated with your IAM users. IAM facilitates this process by allowing two active access keys at a time.
