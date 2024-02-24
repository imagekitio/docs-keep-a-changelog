# Set up SSO for ImageKit on Okta

There are two main steps required to set up SSO using Okta on ImageKit:

1. [Create an Okta application](#create-an-okta-application)
1. [Enable SSO login on ImageKit](#enable-sso-login-on-imagekit)

{% hint style="info" %}
**Okta subscription**\
Although you may use a free developer account on Okta to set up and test the SSO application, having a premium Okta subscription is recommended for seamless role provisioning for your users. Read more [here](#attributes-and-claims).
{% endhint %}


## Create an Okta application

First, you need to create an application on Okta and generate an IdP Metadata XML file.

You may refer to the official documentation by Okta [here](https://help.okta.com/en/prod/Content/Topics/Apps/Apps_App_Integration_Wizard_SAML.htm) or follow our brief guide below:

1. Log in to the [Okta portal](https://www.okta.com/) and open the organization admin panel
1. Navigate to the "Applications" screen using the side navigation menu
1. Click the "Create App Integration" button
1. In the modal popup that opens, choose the "SAML 2.0" radio button as shown below, then click "Next"
1. Input a name for the application, we will use "ImageKit" for this guide, then click "Next"

![Create a SAML 2.0 application](<../../.gitbook/assets/sso-setup-okta-1.png>)
![Single sign-on method](<../../.gitbook/assets/sso-setup-okta-2.png>)

### Configure single sign-on options

#### Basic SAML configuration

On the next screen, we will configure various authentication parameters as shown:

| **Field**                       | **Value**                                      |
| ------------------------------- | ---------------------------------------------- |
| Single sign-on URL              | `https://imagekit.io/saml/consume`             |
| Audience URI                    | `https://imagekit.io/saml/consume`             |
| Default RelayState              | `https://imagekit.io/dashboard`                |
| Name ID format                  | `EmailAddress`                                 |
| Application username            | `Email`                                        |
| Update application username on  | `Create and update`                            |

**Note:** Name ID is the unique email address of the user that will be used to identify them on ImageKit.

![Basic SAML configuration](<../../.gitbook/assets/sso-setup-okta-3.png>)

#### Attributes and claims

Now you need to specify three more keys that ImageKit uses to authorize and provision your users:

| **Field**                   | **Description**                                         | **Claim composition**                                        |
| ---------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| imagekit_id            | The ImageKit ID of your organization account.  | `<your_imagekit_id>`                                                                                 |
| full_name              | The full name of the user. It can be a combination of their first name and last name in the Okta universal directory.  | e.g. `appuser.full_name` which should internally map to `user.firstName + " " + user.lastName`                                                           |
| imagekit_role          | <p>The role to assign to the user on ImageKit that would decide their access privileges.<br></p><p></p><p>Accepted values of this key in the SAML response sent to ImageKit are: </p><p></p><p><ul><li><code>account_administrator</code></li><li><code>developer</code></li><li><code>media_library_full_access</code></li><li><code>media_library_view_only_access</code></li><li><code>media_library_restricted_access</code></li><li><code>finance</code></li></ul></p><p></p><p>Read more about different ImageKit roles and their privileges [here](../user-access-management.md#user-roles).</p> | <p><code>user.<your_custom_attribute></code> OR a custom transformation, as per your Okta user mapping.<br></p><p></p><p>The final computed value of this claim **must** be one of the accepted role strings from the list specified alongside.</p>      |

To do this, you need to create "Profile Mappings" on Okta. Read the official guide [here](https://help.okta.com/oie/en-us/Content/Topics/Security/idp-config-ud-mappings.htm) or follow the quick version below.

Navigate to "Directory > Profile Editor", and create user attributes that will be mapped and sent to ImageKit during authentication. 

![Attributes and claims](<../../.gitbook/assets/sso-setup-okta-4.png>)
![User profile mappings](<../../.gitbook/assets/sso-setup-okta-5.png>)

Back on the SSO application page under the SAML section, ensure that these fields are included correctly in your Okta user object.

![SAML attribute statements](<../../.gitbook/assets/sso-setup-okta-7.png>)

Assign the application to users as shown below to finish this step.

![Assign app to users](<../../.gitbook/assets/sso-setup-okta-10.png>)
![Confirm user profile provisions](<../../.gitbook/assets/sso-setup-okta-11.png>)

### IdP Metadata XML

Navigate to the SAML setup instructions screen and scroll to the section with the **IdP Metadata XML** file. Copy and save it in a safe location. You will need to upload this XML file to your ImageKit account in a later step.

![Find the IdP Metadata XML](<../../.gitbook/assets/sso-setup-okta-8.png>)
![Copy and save the IdP Metadata XML](<../../.gitbook/assets/sso-setup-okta-9.png>)

### Assign users to the SSO application on Okta

Refer to the official Okta guide to [assign the ImageKit application to Okta users](https://help.okta.com/oie/en-us/Content/Topics/users-groups-profiles/usgp-assign-apps.htm).


## Enable SSO login on ImageKit

![Enable SSO for all users](<../../.gitbook/assets/sso-config-screen.png>)

If you have administrator privileges on your ImageKit account, you can enable SSO for all the users in your account as follows:

1. Navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on). 
1. Open the IdP Metadata XML file (which was downloaded [previously](#idp-metadata-xml)) in a text editor of your choice. 
1. Copy and paste the entire contents of the file into the Metadata XML input box.
1. Click on the 'Save' button.

Your users should now be able to use Okta SSO to log into ImageKit.

### First-time login

SSO users would need to initiate their very first login on ImageKit through the ImageKit app by navigating to their end-user dashboard on Okta.

![First-time login](<../../.gitbook/assets/sso-setup-okta-12.png>)

After their first login, they may use the [ImageKit SSO login page](https://imagekit.io/single-sign-on) for signing in to ImageKit directly. Read more [here](README.md#register-a-new-user-on-imagekit-using-sso).

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)

### Disable SSO login on ImageKit

You can disable SSO login for the users on your ImageKit account by deleting the Metadata XML. 

To do so, navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on) and click on the 'Delete' button.


## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
