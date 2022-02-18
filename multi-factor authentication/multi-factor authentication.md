# Multi-factor authentication

Multi-factor authentication adds a layer of protection to the log-in process. A one-time password is generated and sent to the user's registered email address, which is required for additional identity verification. ImageKit.io provides two ways to achieve it:

## On a global level

[Account administrator](../features/user-access-management.md#account-administrator) can enforce MFA for all users related to the ImageKit account (including self). If admin has enforced MFA for whole account, then a non-admin user cannot disable it and is forced to authenticate using MFA on next login attempt.

To enable/disable MFA for whole account, account administrator can go to [multi-factor authentication setting](https://imagekit.io/dashboard/settings/multi-factor-auth) in the ImageKit.io dashboard.

![MFA for whole account in ImageKit.io dashboard](<../.gitbook/assets/settings-multi-factor-authentication.png>)

## On a personal level

One can setup MFA on their account for additional security.

To enable/disable MFA for your account, you can go to [profile section](https://imagekit.io/dashboard/profile) in the ImageKit.io dashboard.

![MFA toggle in profile in ImageKit.io dashboard](<../.gitbook/assets/profile-multi-factor-authentication.png>)

To know more about user roles, go to [User roles](../features/user-access-management.md#user-roles).