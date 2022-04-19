# Set up SSO for ImageKit on Azure

There are two main steps required to set up SSO using Azure on ImageKit:

1. [Create an Azure AD application](#create-an-azure-ad-application)
1. [Enable SSO login on ImageKit](#enable-sso-login-on-imagekit)

{% hint style="info" %}
**Role provisioning ** \
Although you may use a free plan on Azure to set up and test the SSO application, having a premium Azure subscription is recommended for seamless role provisioning for your users. Read more [here](#attributes-and-claims).
{% endhint %}


## Create an Azure AD application

First, you need to create an application on Azure Active Directory and generate a Federation Metadata XML file.

You may refer to the official documentation by Microsoft [here](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/configure-single-sign-on-portal) or follow our brief guide below:

1. Log in to [Microsoft Azure portal](https://portal.azure.com) and open Azure Active Directory
1. Navigate to the "Enterprise applications" screen using the side navigation menu
1. Click the "+ New application" button, then click on "+ Create your own application"
1. In the form that opens up, choose the "Non-gallery application" radio button as shown below. 
1. Input a name for the application, we will use "ImageKit" for this guide.

Click "Create", and wait till you are redirected to the application page.

![Create a non-gallery application](<../../.gitbook/assets/sso-setup-azure-1.png>)

### Configure single sign-on options

On the application page, navigate to the 'Single sign-on' screen. Select "SAML" as the single sign-on method.

![Single sign-on method](<../../.gitbook/assets/sso-setup-azure-2.png>)

#### Basic SAML configuration

On the next screen, we will configure various authentication URLs as shown:

| Name                  | Value                                          |
| --------------------- | ---------------------------------------------- |
| Identifier            | `https://imagekit.io/saml/consume`             |
| Reply URL             | `https://imagekit.io/saml/consume`             |
| Relay State           | `https://imagekit.io/dashboard`                |
| Logout Url            | `https://imagekit.io/logout`                   |


![Basic SAML configuration](<../../.gitbook/assets/sso-setup-azure-3.png>)

#### Attributes and claims

Now you need to specify four keys that ImageKit uses to authorize and provision your users:

| Name                         | Description                                         | Claim composition                                                                          |
| ---------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Unique User Identifier       | The unique email address of the user that will be used to identify them on ImageKit. | `user.mail`                                                                                          |
| imagekit_id                  | The ImageKit ID of your organization account.  | `<your_imagekit_id>`                                                                                 |
| full_name                    | The full name of the user. It can be a combination of their given name and surname on Azure AD.  | `Join (user.givenname, " ", user.surname)`                                                           |
| imagekit_role                | <p>The role to assign to the user on ImageKit which would decide their access privileges.</p><p><p>Accepted values of this key in the SAML response sent to ImageKit are: <ul><li><code>account_administrator</code></li> <li><code>developer</code></li> <li><code>media_library_full_access</code></li> <li><code>media_library_view_only_access</code></li><li><code>finance</code></li></ul></p><p></p></p><p>Read more about different ImageKit roles and their privileges [here](../user-access-management.md).</p> | <p><code>user.<your_custom_attribute></code> OR a custom transformation, as per your Azure user schema.</p><p></p><p>The final computed value of this claim **must** be one of the accepted role strings from the list specified alongside.</p>      |

For this guide, we will map "imagekit_role" to `user.department` Azure key. Ensure that this field is populated correctly in your Azure user object while testing the app.

Save the list of attributes and claims to finish this step.

![Attributes and claims](<../../.gitbook/assets/sso-setup-azure-4.png>)

### Federation Metadata XML

Download the **Federation Metadata XML** file and keep it in a safe location. You will need to upload this XML file to your ImageKit account in a later step.

![Download Federation Metadata XML](<../../.gitbook/assets/sso-setup-azure-5.png>)

### Assign users to the SSO application on Azure

Refer to the official Microsoft guide to [assign users to the ImageKit application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/add-application-portal-assign-users).


## Enable SSO login on ImageKit

![Enable SSO for all users](<../../.gitbook/assets/sso-config-screen.png>)

If you have administrator privileges on your ImageKit account, you can enable SSO for all the users in your account as follows:

1. Navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on). 
1. Open the Federation Metadata XML file (which was downloaded [previously](#federation-metadata-xml)) in a text editor of your choice. 
1. Copy and paste the entire contents of the file into the Metadata XML input box.
1. Click on the 'Save' button.

Your users should now be able to use Microsoft Azure SSO to log into ImageKit. You can verify this by clicking the 'Test' button on the 'Single sign-on' set up screen on Azure.

### First-time login

SSO users would need to initiate their very first login on ImageKit through the Azure app by navigating to their [My Apps](https://myapps.microsoft.com/) page.

![First-time login](<../../.gitbook/assets/sso-setup-azure-6.png>)

After their first login, they may use the [ImageKit SSO login page](https://imagekit.io/single-sign-on) for signing in to ImageKit directly. Read more [here](README.md#register-a-new-user-on-imagekit-using-sso).

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)

### Disable SSO login on ImageKit

You can disable SSO login for the users on your ImageKit account by deleting the Metadata XML. 

To do so, navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on) and click on the 'Delete' button.


## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
