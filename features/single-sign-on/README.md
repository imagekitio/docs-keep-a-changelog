# Single sign-on

{% hint style="info" %}
**Paid plan only**\
This feature is available only on custom enterprise pricing plans.
{% endhint %}

{% hint style="info" %}
[Enforce multi-factor authentication (MFA)](../multi-factor-authentication.md) setting is not applicable for users logging in via single sign-on (SSO) authentication and enabling/disabling it will have no effect on the authentication flow of the user to Imagekit's dashboard. If you wish to enable MFA, you may do so in your Identity Provider (IdP) platform.
{% endhint %}

Single sign-on (SSO) is an authentication method that allows users to log in to ImageKit with a single unique ID provided through a trusted Identity Provider (IdP).

SSO adds a layer of centralized control to user management. You can manage all the users in your organization and assign their ImageKit role from a central Identity Provider (IdP) platform. ImageKit will use that IdP to register, authenticate and authorize your users.

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)


## How to set up SSO?

If you have administrator privileges on your ImageKit account, you can set up SSO for all the users in your account by following these guides for supported Identity Providers:

- [Set up SSO for ImageKit on Azure](sso-azure-setup.md)
- [Set up SSO for ImageKit on Okta](sso-okta-setup.md)
- [Set up SSO for ImageKit on OneLogin](sso-onelogin-setup.md)
- [Set up SSO for ImageKit on AD FS](sso-adfs-setup.md)


## How does ImageKit use SSO?

ImageKit uses SAML-based authentication for "Just In Time" (JIT) provisioning for your users. JIT provisioning means that you do not need to create or update users on the ImageKit [User management](../user-access-management.md) page manually. This will be done automatically whenever a user that exists on your IdP logs into ImageKit using SSO.

For this, you need to configure SAML provisions on your Identity Provider platform.

### Register a new user on ImageKit using SSO

Users on your IdP that do not yet have an account on ImageKit must initiate authentication through the SSO application on your IdP portal for the first time. 

This process will provide ImageKit with the information needed to register that user on ImageKit and associate them with your organization account.

Upon successful authentication from the IdP, ImageKit will parse the SAML response. If a user with the unique email ID provided in the response does not already exist on ImageKit, a new user having the specified email ID, full name and ImageKit role will be registered under your organization account.

### Log in and update an existing user on ImageKit using SSO

Registered users can log in to ImageKit using SSO on [this page](https://imagekit.io/single-sign-on). Once they enter their registered email ID and submit the form, they will be redirected to the associated IdP for authentication. 

Alternatively, they may use the SSO application on your IdP portal to initiate authentication directly.

Upon successful authentication response from the IdP, ImageKit will check if there have been any changes to the user's information (such as full name and ImageKit role) since their last login on ImageKit. If so, the information will be updated on ImageKit automatically. 

### SSO and Multi-factor authentication (MFA)

Since the primary authentication of SSO users is provided by the Identity Provider (IdP), multi-factor authentication for these users is also best configured through the IdP. You can follow the documentation of your configured IdP to set it up on its platform. 

Even if an SSO user enables MFA on their own profile through the User profile page or an administrator enforces MFA for all users through the User Management Page, no unique code will be sent to the email associated with the SSO user's account and user will be redirected to Imagekit dashboard following successful authentication by IdP.

### Delete an existing user

When a user is deleted from your IdP, they would be unable to log in to ImageKit using SSO. However, their account would still exist on ImageKit and count towards your subscription plan. To avoid this, you should delete them immediately from the User Management page on ImageKit as well.


## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
