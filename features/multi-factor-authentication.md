# Multi-factor authentication

You must be using ImageKit on your live websites and apps. Multiple users might access the ImageKit dashboard, the central place for all your media uploads, external origin configurations, delivery settings, and more. Keeping the dashboard secure therefore is of utmost importance.

While having a strong password helps secure access to the dashboard, multi-factor authentication makes it safer.

Multi-factor authentication (MFA) adds a layer of protection to the log-in process. A one-time password is generated and sent to the user's registered email address, which is required for additional identity verification. 

## How to enable MFA?

### Enforce MFA for all users

If you have administrator privileges on your ImageKit account, you can enforce MFA for all the users in your account from the [User Management page](https://imagekit.io/dashboard/user-management). 

This setting cannot be overridden by any user on their profile.

![Enforce MFA for all users](<../.gitbook/assets/mfa-enforce-user-management.png>)

### Enable MFA on your profile

Every user can enable MFA for their account from their [User Profile page](https://imagekit.io/dashboard/profile). 

This option is unavailable if an account administrator has enforced MFA for all users belonging to that account.

![MFA for your account](<../.gitbook/assets/mfa-user-profile.png>)

## How does MFA in ImageKit work?

Once MFA is enabled for your account, either by the account administrator or on your own, ImageKit would send a unique code to the email ID associated with your account after you enter the correct login password.

![MFA screen](<../.gitbook/assets/mfa-otp-screen.png>)

You will need to provide this unique code in the form that comes up on the next screen to access the dashboard.

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
