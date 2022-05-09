# Single sign-on

{% hint style="info" %}
**Paid plan only**\
At the moment, this feature is available in all paid plans. But soon, it will be only available on custom enterprise pricing plans.
{% endhint %}

Single sign-on (SSO) is an authentication method that allows users to log in to ImageKit with a single unique ID provided through a trusted Identity Provider (IdP).

SSO adds a layer of centralized control to user management. You can manage all the users in your organization and assign their ImageKit role from a central Identity Provider (IdP) platform. ImageKit will use that IdP to register, authenticate and authorize your users.

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)


## How to set up SSO?

If you have administrator privileges on your ImageKit account, you can set up SSO for all the users in your account by following these guides for supported Identity Providers:

- [Set up SSO for ImageKit on Azure](sso-azure-setup.md)


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

### Delete an existing user

When a user is deleted from your IdP, they would be unable to log in to ImageKit using SSO. However, their account would still exist on ImageKit and count towards your subscription plan. To avoid this, you should delete them immediately from the User Management page on ImageKit as well.


## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
