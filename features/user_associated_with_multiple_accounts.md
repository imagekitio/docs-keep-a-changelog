# User associated with multiple accounts

A user can be associated with multiple ImageKit accounts using the same email address. A previously registered user can be added to a different account. Read how to add a user to an account [here](./user-access-management/README.md).

The login flow for a multi-account user is as follows:

You will be asked to put in your email and password (any valid password for one of the accounts) at the login screen.

![Login screen](../.gitbook/assets/login_screen.png)

Then you will be redirected to the following page and asked to put in a one-time password (OTP) sent to the associated email ID.

![OTP input screen](../.gitbook/assets/OTP_input_screen.png)

Upon successful validation of the OTP provided, you will select the imagekit ID of the account you want to access on the next screen.

![Account selection screen](../.gitbook/assets/account_selection_screen.png)

## Multi-factor authentication (MFA) for multi-account users

For users that are added to multiple ImageKit accounts, login always requires a one-time password (OTP) that is sent to the registered email.

[Enforce MFA](./multi-factor-authentication.md) setting does not apply to such users. They will always be required to enter the OTP to login to the dashboard.

## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
