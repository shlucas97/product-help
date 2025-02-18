---
title: Managing Users
hide_title: true
sidebar_position: 3
---
# Managing Cado Users
In the Cado platform you can grant access only to the data to which they need access - to both processed data within the platform, and resources or raw data residing in the cloud. 

### User Types
There are two roles in the Cado platform. 

| Name | Description |
| ---- | ----------- |
| Administrator | Can edit users and access all projects and configured cloud resources. |
| Normal User | Users with restricted access to a subset of projects and cloud resources. |

In order to get access to projects and data a Normal User needs to be added to a project, or a group that has access to that project. In order to acquire cloud data for a project, the user needs to be given access to a CSP Role that has access to that cloud data or resource, or added to a group that has access to that CSP Role


![Users-Groups-Roles](/img/users-groups-roles.png)

### Configuring Single Sign On (SSO)
Cado also supports authentication via [Azure AD](sso/azure-ad.md), Okta ([OAuth](sso/okta.md) or [SAML](sso/okta_saml.md)) and [PingID](sso/ping_saml.md). When you configure SSO access, the Cado platform will automatically create the user at first login.

### Managing Roles
Roles in Cado correspond to CSP roles in AWS, Azure or GCP that have appropriate levels of access to cloud resources. Only Administrators can manage roles. This list is autopopulated when administrators add CSP credentials to the platform following the instructions for [AWS](/cado-response/deploy/aws/iam/cross-account-creation#adding-the-role-to-cado), [Azure](/cado-response/deploy/azure/azure-cross-tenancy-subscriptions#registering-credentials-within-cado), and [GCP](/cado-response/deploy/gcp/gcp-settings#entering-settings) respectively.

### Managing Groups
Groups in Cado allow you to define groups of users that you can use to assign or revoke access to projects and/or cloud resources. Only Administrators can manage groups. 

To create a new group:
- Click **Groups**
- Click the **Add Group** button
- Enter the name of the group
- Enter the name of a group in your SSO platform that corresponds to this group. When selected, any members of your SSO group that log into the Cado Platform will automatically be joined to this group
- Select any users that need to assigned to this group
- Select any CSP Roles that users in this group need access to

![Groups](/img/groups.png)

### Creating a New User
Only Administrators can create new users.  When an Administrator creates a new user, a temporary password must be created by the Administrator.  The new user will be asked to change their password when they first login.
To add a new user:
- Click **Users** 
- Click the **Add Users** button
- Select any groups the user needs to be assigned
- Select any CSP Roles the user need to be assigned

### Granting Administrator Access
To elevate privileges and grant Administrator access to a normal user, do the following:
- Click **Users**
- Next to the appropriate user, click the Edit icon ![Edit](/img/edit.png)
- Select the **This user has administrator access** checkbox
- Click **Update**

:::caution
It is strongly recommended to follow the principles of least privilege when creating new users and granting Administrator access.
:::

### Granting Access to a Project
To grant existing users or groups to a Project, you can add them when you create the Project, or you can follow the below instructions to add users to an existing Project:
- Click **Projects** and select the project to which you would like to add users
- Click the **Access** button 
- Click the **Add Users** button
- Select the user and/or group to add and Click **Add**
