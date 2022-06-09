# Set up SSO for ImageKit on OneLogin

There are two main steps required to set up SSO using OneLogin on ImageKit:

1. [Create a OneLogin application](#create-an-onelogin-application)
1. [Enable SSO login on ImageKit](#enable-sso-login-on-imagekit)

{% hint style="info" %}
**OneLogin subscription**\
Although you may use a free account on OneLogin to set up and test the SSO application, having a premium OneLogin subscription is recommended for seamless role provisioning for your users. Read more [here](#attributes-and-claims).
{% endhint %}


## Create a OneLogin application

First, you need to create an application on OneLogin and generate a SAML Metadata XML file.

You may refer to the official documentation by OneLogin [here](https://developers.onelogin.com/quickstart/saml) or follow our brief guide below:

1. Log in to the [OneLogin portal](https://www.onelogin.com/) and open the Administration panel
1. Navigate to the "Applications" screen using the top navigation menu
1. Click the "Add App" button, then search for "SAML Custom Connector"
1. Choose the correct "Advanced" connector as shown below
1. Input a name for the application, we will use "ImageKit" for this guide

Click "Save" and wait until you are redirected to the application page.

![Create a SAML application](<../../.gitbook/assets/sso-setup-onelogin-1.png>)
![Choose the SAML Custom Connector (Advanced)](<../../.gitbook/assets/sso-setup-onelogin-2.png>)

### Configure single sign-on options

On the application page, navigate to the 'Configuration' screen. 

#### Basic SAML configuration

On this screen, we will configure various authentication parameters:

| **Field**                     | **Value**                                      |
| ----------------------------- | ---------------------------------------------- |
| RelayState                    | `https://imagekit.io/dashboard`                |
| Audience (EntityID)           | `https://imagekit.io/saml/consume`             |
| Recipient                     | `https://imagekit.io/saml/consume`             |
| ACS (Consumer) URL Validator  | `https://imagekit.io/saml/consume`             |
| ACS (Consumer) URL            | `https://imagekit.io/saml/consume`             |
| Single Logout Url             | `https://imagekit.io/logout`                   |
| SAML initiator                | `OneLogin`                                     |
| SAML nameID format            | `Email`                                        |

Leave the rest of the values as they are.

**Note:** Name ID is the unique email address of the user that will be used to identify them on ImageKit.

![Basic SAML configuration](<../../.gitbook/assets/sso-setup-onelogin-3.png>)

#### SSO signature settings

Navigate to the "SSO" tab and change the value of "SAML Signature Algorithm" to `SHA-256`. 

You may also note an alternate link to obtain SAML Metadata XML below it.

![SSO signature settings](<../../.gitbook/assets/sso-setup-onelogin-8.png>)

#### Attributes and claims

Now you need to specify three custom keys that ImageKit uses to authorize and provision your users:

| **Field**                   | **Description**                                         | **Claim example**                                        |
| ---------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| imagekit_id            | The ImageKit ID of your organization account.  | `<your_imagekit_id>`                                                                                 |
| full_name              | The full name of the user. | e.g. `Jade Smith`                                                           |
| imagekit_role          | <p>The role to assign to the user on ImageKit that would decide their access privileges.<br></p><p></p><p>Accepted values of this key in the SAML response sent to ImageKit are: </p><p></p><p><ul><li><code>account_administrator</code></li><li><code>developer</code></li><li><code>media_library_full_access</code></li><li><code>media_library_view_only_access</code></li><li><code>finance</code></li></ul></p><p></p><p>Read more about different ImageKit roles and their privileges [here](../user-access-management.md#user-roles).</p> | <p>e.g. <code>developer</code><br></p><p></p><p>The final computed value of this claim **must** be one of the accepted role strings from the list specified alongside.</p>      |

To do this, you need to create "Custom User Fields" on OneLogin. Read the official guide [here](https://developers.onelogin.com/api-docs/1/users/set-custom-attribute) or follow the quick version below.

Navigate to "Users > Custom User Fields", and create user attributes that will be mapped and sent to ImageKit during authentication. 

![Custom User Fields](<../../.gitbook/assets/sso-setup-onelogin-9.png>)

Back on the SSO application page under the SAML section, ensure that these fields are included correctly in your OneLogin user object.

![Attach claims](<../../.gitbook/assets/sso-setup-onelogin-5.png>)
![Attach claims](<../../.gitbook/assets/sso-setup-onelogin-6.png>)
![Attach claims](<../../.gitbook/assets/sso-setup-onelogin-7.png>)
![SAML attribute statements](<../../.gitbook/assets/sso-setup-onelogin-10.png>)


### Assign users to the SSO application on OneLogin

Assign users to the ImageKit application by going to the Users section, selecting a user, and navigating to the Applications tab on the user's details page.

Save your assignments before proceeding further.

![Assign a user to the app](<../../.gitbook/assets/sso-setup-onelogin-11.png>)


### SAML Metadata XML

Navigate to the application setup screen and use the popover menu highlighted here to download the **SAML Metadata XML** file. 

Save it in a safe location. You will need to upload this XML file to your ImageKit account in a later step.

![Download and save the SAML Metadata XML](<../../.gitbook/assets/sso-setup-onelogin-4.png>)


## Enable SSO login on ImageKit

![Enable SSO for all users](<../../.gitbook/assets/sso-config-screen.png>)

If you have administrator privileges on your ImageKit account, you can enable SSO for all the users in your account as follows:

1. Navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on). 
1. Open the SAML Metadata XML file (which was downloaded [previously](#issuer-metadata-xml)) in a text editor of your choice. 
1. Copy and paste the contents of the file into the Metadata XML input box.
1. Click on the 'Save' button.

Your users should now be able to use OneLogin SSO to log into ImageKit.

### First-time login

SSO users would need to initiate their very first login on ImageKit through the ImageKit app by navigating to their dashboard on OneLogin.

![First-time login](<../../.gitbook/assets/sso-setup-onelogin-12.png>)

After their first login, they may use the [ImageKit SSO login page](https://imagekit.io/single-sign-on) for signing in to ImageKit directly. Read more [here](README.md#register-a-new-user-on-imagekit-using-sso).

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)

### Disable SSO login on ImageKit

You can disable SSO login for the users on your ImageKit account by deleting the Metadata XML. 

To do so, navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on) and click on the 'Delete' button.


## Support

If you face any issues while using these features or have a question or suggestion, please reach out to us at support@imagekit.io.
