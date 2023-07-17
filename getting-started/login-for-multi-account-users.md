# Login for multi-account users

You can have multiple Imagekit accounts associated with the same email. For a multi-account user, the login flow will be as follows:


You will be asked to put in your email and password (any valid password for one of the accounts) at the login screen.

![Login screen](<../.gitbook/assets/login_screen.png>)


Then you will be redirected to the following page and asked to put in a one-time password (OTP) sent to the associated email ID.


![OTP input screen](<../.gitbook/assets/OTP_input_screen.png>)


Upon successful validation of the OTP provided, you will select the imagekit ID of the account you want to access on the next screen.


![Account selection screen](<../.gitbook/assets/account_selection_screen.png>)


### Multi-factor authentication (MFA) for multi-account users


[Enforce multi-factor authentication (MFA)](../multi-factor-authentication.md) setting is not applicable for multi-account users, and enabling or disabling it will have no effect on the authentication flow of the multi-account user to Imagekit's dashboard. Since the multi-account user will always be asked to provide the one-time password (OTP) sent to the associated email ID before accessing the account selection form, sending another unique code for MFA to the same associated email ID and validating it on the next screen seems redundant and is therefore skipped.


If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.