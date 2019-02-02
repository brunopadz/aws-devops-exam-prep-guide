# Delegation and Federation

* Delegation is when you allow users from another AWS account to access/use resources in your account
* Federation allows users from external Identity Providers access to your account
* There are 2 types of federation:  
    * Enterprise Identity Federation; and use sources such as AD, LDAP and custom federation proxy, SAML or AWS Directory Service
    * Social Identity Federation; trust a web based Identity Provider such as Amazon, Facebook, Google, OpenID Connect.

### Roles

* Role is an object which contains 2 policy documents, the trust policy and the access policy
    * The trust policy states what AWS account can be trusted to assume a role.
    * The access policy states what permission will be granted

Example of trust policy
```json
{
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam:123123123123:root"
    },
    "Action": "sts:AssumeRole"
}
```

Example of access policy
```json
{
    "Effect": "Allow",
    "Action": "*",
    "Resource": "*"
}
```

### Sessions

* Sessions are a set of temporary credentials - access and secret key with an expiration
* These credentials are obtained via Security Token Service (STS)
    * `sts:AssumeRole`. `sts:AssumeRoleWithSAML` or `sts:AssumeRoleWithWebIdentity`

### Service Delegation

* IAM Roles are attached to EC2 and Lambda allows you to grant a role to a Lambda function. These services constantly uses `sts:AssumeRole`, obtaining credentials and making them available to the instance or function via metadata.

## Corporate Identity Federation

* Allows you to use an existing identity store for AWS access.
* Identity stores can be:
    * AWS Directory Services
    * SAML compatible providers (AD)
    * Custom federation proxy
* Temporary access is provided by Security Token Service and access is obtained via the `GetFederationToken` or `sts:AssumeRole` operations

### How it works

* STS provides you with session credentials, AKID, secret access key, session token and expiration
    * Expiration values are min, max and default
        * When using `sts:AssumeRole` the values are: 15 minutes, 1 hour, 1 hour
        * When using `GetFederationToken` the values are: 15 minutos, 36 hours, 12 hours
* Generally there's a group mapping from Identity Provider with Roles inside AWS

## Web Identity Federation

* Allows a trusted 3rd party to authenticate users. 
* Avoids the need to create and manage users in IAM and multiple IDs
* Simplifies access control via roles
* Improves security, since no permanent credentials will be store in applications

### Cognito

* Cognito is an identity management and sync service
    * Cognito Identity handles the web identity within AWS services
    * Cognito Sync which handles user synchronization across devices
* Cognito introduces the concept of an identity pool
    * Identity pool is a collection of identities, it allows grouping of identities from different providers as a single entity
* There are 4 types of auth flow inside Cognito:
    * Pre-Cognito
    * Unauthenticated or Guest
    * Simple (classic)
    * Enhanced (simplified)

