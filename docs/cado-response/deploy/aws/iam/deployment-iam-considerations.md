---
title: Overview
hide_title: true
sidebar_position: 1
---
# Deployment IAM Considerations
There are several AWS Identity & Access Management best practices to consider when deploying the Cado platform 

## Dedicated Forensics Account
You may choose to deploy into an AWS account dedicated to storing forensics data securely. You can then use cross-account roles to bring data into the forensics account. Cado copies data back into the forensics account, and performs processing there.

## Cross-Account Access without the Cross-Account Role
By default, we recommend creating a role in each account that you want Cado to access. This enables one click acquisition of data.
However, if you cannot create roles in other accounts you can still use the AMI import functionality to import EC2 resources from other accounts without the need to create any cross-account roles. This requires you to create the AMI of any instance you want to import yourself and share it with the AWS account that Cado resides in.

## Removing and Tightening IAM Permissions
You can further tune the IAM permissions that Cado deploys if you do not require all functionality. We describe the functionality used by the permissions in the “Sid” section of the [cross-account role](https://cado-public.s3.amazonaws.com/policy-in-cross-account.json). Please contact support@cadosecurity.com for advice on what permissions are required for.
