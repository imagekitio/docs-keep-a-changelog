# Set up SSO for ImageKit on AD FS

There are two main steps required to set up SSO using AD FS on ImageKit:

1. [​Create an AD FS relying party trust](#create-an-ad-fs-relying-party-trust-rp)
1. [Enable SSO login on ImageKit](#enable-sso-login-on-imagekit)

This guide is based on a standard AD FS installation with publicly accessible Identity Provider (IdP) endpoints.

## Create an AD FS Relying Party trust (RP)

First, you need to create a Relying Party (RP) on AD FS:

1. Open the AD FS console and click on "Add relying party trust" in the action panel.
1. Choose "Claims aware" and click "Start".
1. Select "Enter data about the relying party manually" and click "Next".
1. Enter "ImageKit" as a display name, and click "Next".
1. Select "AD FS profile" and click "Next".
1. A token encryption cert is unnecessary, so click "Next".
1. Check the box "Enable support for the SAML 2.0 WebSSO protocol", 
1. The relying party SAML 2.0 SSO service URL box should be `https://imagekit.io/saml/consume` - click "Next".
1. Configure multi-factor authentication or skip if not required.
1. Select which users you permit to access ImageKit, and click "Next". 
1. Review, click Next, then close the wizard.


### Configure claims issuance policy

Now you need to configure the claims to be issued to ImageKit as SAML assertions. 

ImageKit uses the following four values to authorize and provision your users:

1. `Name ID` – The user's unique email address, which is used to identify them on ImageKit. 
1. `imagekit_id` – The ImageKit ID of your organization account.
1. `full_name` – The full name of the user. It can be a combination of their given name and surname or their display name on AD FS. 
1. `imagekit_role` – The role to assign to the user on ImageKit, which would decide their access privileges. Read about different ImageKit roles and their privileges [here](https://docs.imagekit.io/features/user-access-management#user-roles). Accepted values of this key in the SAML response sent to ImageKit are:
    - `account_administrator`
    - `developer`
    - `media_library_full_access`
    - `media_library_view_only_access`
    - `finance`

To configure these values, click "Edit Claim Issuance Policy" and perform the following steps:

#### `Name ID` and `full_name`

1. Choose the Issuance Transform Rules tab and click "Add Rule".
1. Select Send LDAP Attributes as Claims and click "Next".
1. Enter a Claim rule name, such as "User attributes", then set the Attribute store to Active Directory, type in E-Mail-Addresses for the first LDAP attribute, and set its outgoing type to Email Address. 
1. Similarly, choose an applicable value such as "Display name" for `full_name`.
1. Click Finish when you are done.
1. Click "Add Rule" on the Issuance Transform Rules tab again.
1. Select Transform an Incoming Claim and click Next.
1. Enter a Claim rule name, such as Name ID Transform, set the Incoming claim type to Email Address, set the Outgoing claim type to Name ID, and set the Outgoing name ID format to Email. Select Pass through all claim values and click Finish.
1. Click OK on the Edit Claim Rules dialog.

![LDAP attributes](<../../.gitbook/assets/sso-setup-adfs-1.png>)

#### `imagekit_id`

This is the ImageKit ID of your organization account, obtained from the Developer options page.

1. Choose the Issuance Transform Rules tab and click "Add Rule".
1. Select "Send claims using a custom rule" and click "Next".
1. Enter the Claim rule name as imagekit_id
1. Set custom rule as: `=> issue(Type = "imagekit_id", Value = "<your_imagekit_id>");`
1. Click "Ok".

![ImageKit ID](<../../.gitbook/assets/sso-setup-adfs-2.png>)

#### `imagekit_role`

The final computed value of this claim must be one of the accepted role strings from the list specified earlier. Read about ImageKit roles and their privileges [here](https://docs.imagekit.io/features/user-access-management#user-roles). 

**In the AD FS user management panel:**

Create five different groups for each of these roles. You can name them as you wish. For this guide, we have mapped groups to `imagekit_role` values as follows:

| **Group name**                 | **Outgoing claim value** (`imagekit_role value`) |
| ------------------------------ | ------------------------------------------------ |
| Account Administrator          | `account_administrator`                          |
| Developer                      | `developer`                                      |
| Media Library Full Access      | `media_library_full_access`                      |
| Media Library View Only Access | `media_library_view_only_access`                 |
| Finance                        | `finance`                                        |

Assign your users to these groups as applicable. Ideally, one user should have only one of the roles from the above list.

**On the AD FS relying party screen:**

We shall now create claim mappings for each of these roles. First, navigate to the relying party screen and click on "Edit Claim Issuance Policy". 

Let's start with the `account_administrator` role:

1. Choose the Issuance Transform Rules tab and click "Add Rule".
1. Use the AD FS User Groups to determine `imagekit_role`. Select "Send Group Membership as a Claim" and click "Next".
1. Enter the Claim rule name as `Admin role`. Next, select the applicable group `Account Administrator`. 
1. Set outgoing claim type as `imagekit_role`. Enter the claim value as `account_administrator` for this group.
1. Click "Apply".

![Group claim configuration](<../../.gitbook/assets/sso-setup-adfs-3.png>)

![Admin role group](<../../.gitbook/assets/sso-setup-adfs-4.png>)

**Repeat the above steps for each role and group.**

e.g., For a developer:

![Developer role group](<../../.gitbook/assets/sso-setup-adfs-5.png>)

Once all the roles are configured correctly, the claims issuance policy should look similar to this:

![Claims issuance policy](<../../.gitbook/assets/sso-setup-adfs-6.png>)


### Federation Metadata XML

Download the **Federation Metadata XML** file and keep it in a safe location. You will need to upload this XML file to your ImageKit account in a later step.

It should be available on a link similar to the following:

```
https://<your_adfs_ domain>/federationmetadata/2007-06/FederationMetadata.xml
```

Replace `<your_adfs_domain>` with the domain of your AD FS instance.

### Assign users to Imagekit RP on AD FS

Assign users to the created AD FS relying party trust as you usually would.


## Enable SSO login on ImageKit

![Enable SSO for all users](<../../.gitbook/assets/sso-config-screen.png>)

If you have administrator privileges on your ImageKit account, you can enable SSO for all the users in your account as follows:

1. Navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on). 
1. Open the Federation Metadata XML file (which was downloaded [previously](#federation-metadata-xml)) in a text editor of your choice. 
1. Copy and paste the entire contents of the file into the Metadata XML input box.
1. Click on the 'Save' button.

Your users should now be able to use AD FS SSO to log into ImageKit. 


## User registration on ImageKit

### Option 1 – Create users on ImageKit via AD FS

#### First-time login

SSO users would need to initiate their very first login on ImageKit through the AD FS IdP login page. 

The URL of your AD FS IdP login page must include the `RelayState` query parameter. Here is an example:
```
https://<your_adfs_domain>/adfs/ls/idpinitiatedsignon.aspx?RelayState=RPID%3dhttps%3A%2F%2Fimagekit.io%2Fsaml%2Fconsume%26RelayState%3dhttps%3A%2F%2Fimagekit.io%2Fdashboard
```
Replace `<your_adfs_domain>` with the domain of your AD FS instance.

The encoded `RelayState` query string consists of two nested parameters:

| **Parameter** | **Decoded value**                  | **Purpose**                                                        |
| ------------- | ---------------------------------- | ------------------------------------------------------------------ |
| RPID          | `https://imagekit.io/saml/consume` | Identifies the RelyingParty on AD FS                               |
| RelayState    | `https://imagekit.io/dashboard`    | Redirects the user to the ImageKit dashboard after authentication  |

![First-time login](<../../.gitbook/assets/sso-setup-adfs-7.png>)

After logging in to ImageKit for the first time, registered users may use the ImageKit SSO login page to sign in to ImageKit directly. Read more [here](https://docs.imagekit.io/features/single-sign-on#register-a-new-user-on-imagekit-using-sso).

![ImageKit SSO login screen](<../../.gitbook/assets/sso-login-screen.png>)

### Option 2 – Create users directly on ImageKit

Alternatively, if you have administrator privileges on your ImageKit account, you may add users directly to ImageKit with their respective AD FS email IDs through the [User management page](https://imagekit.io/dashboard/user-management). After email verification, these users may log in using the SSO login page shown above.

## Disable SSO login on ImageKit

You can disable SSO login for the users on your ImageKit account by deleting the Metadata XML. 

To do so, navigate to the [Settings page](https://imagekit.io/dashboard/settings/single-sign-on) and click the 'Delete' button.

## Support

If you face any issues while using these features or have questions or suggestions, please reach out to us at support@imagekit.io.
