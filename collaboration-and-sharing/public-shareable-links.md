# Public shareable links

{% hint style="info" %}
**Enterprise plan only**
This feature is only available in custom enterprise pricing plans.
{% endhint %}

When an asset or media collection needs to be shared externally with someone outside your ImageKit account, a public shareable link can be created for it. Public links allow limited, read-only access to your assets without the need to register for an ImageKit account. This includes the ability to download multiple assets in a ZIP file. A link can be created for a file, folder, or media collection. Configurable options for a public link include password protection, a validity timeframe, and whether to expose [draft assets](../media-library/overview/draft-assets.md) or not.

For restricted media library users, the minimum permission level needed to create, view, and edit public links for an asset or media collection is "Can manage". Learn more about permissions and access sharing for restricted users [here](../collaboration-and-sharing/README.md#understanding-permissions-on-assets-and-media-collections).

{% hint style="info" %}
**Limits**
- For a single asset or media collection, a maximum of 5 public links can be created.
- Across all assets and media collections in an account, a maximum of 100 public links can be created. This limit is configurable.
{% endhint %}

## Create and manage public links

### From the media library
You can create a public link for any asset or media collection in your media library from the right-click menu on the asset or media collection. From the same menu, you can also view and delete existing public links, and change any of its configurable options. Doing this will not change the URL of the link. For a media library restricted user, this option will be available only if the user has "Can manage" permission on the asset.

![Right-click menu location for public links](<../.gitbook/assets/right-click-menu-public-links.png>)

![Form for creating a new public link](<../.gitbook/assets/new-public-link-form.png>)

You can configure any of the following options for a public link:
- Validity timeframe: You can set the "Validity start" and/or "Validity end" dates for any public link. At any time that is either earlier than the "Validity start" time, or later than the "Validity end" time, the link will not work. After a link has expired, it will be automatically deleted.
- Password protection: If a password is set for a public link, any user trying to access the link will be prompted to enter that password before access is granted. This password can be changed anytime without affecting the URL of the public link.
- Expose unpublished assets: When creating a public link for a folder or media collection, this setting defines whether any unpublished assets that fall under that folder or media collection can be accessed via the link or not. If a public link is created for a single file, then the file will be accessible via the link, regardless of its publication status.

### From the settings menu

On the media library settings page, you can view, edit, and delete public links for all assets and media collections in your media library. Media library restricted users do not have access to this page.

![Table displaying all public links in an account](<../.gitbook/assets/all-public-links-table-settings-page.png>)

## Public link view

The view generated via a public link allows limited, read-only access to assets and media collections. Most operations available in the media library will not be available via a public link view. Any changes to an asset or media collection made from the media library, such as moving an asset to a different location, or adding/removing an asset to a media collection, will be immediately reflected in the public link view.

- Private files are always accessible, but whether a file is private or not will not be visible in the view.
- Access to unpublished files depend on the "Expose unpublished files" setting for the public link, but a file's publication status will not be visible in the view.
- Tags, AI-generated gags, Custom metadata, and Comments will not be visible in the view.
- Any information related to a file's versions will not be exposed. Only the current version of a file will be accessible via the view.

![Public links view](<../.gitbook/assets/public-links-view.png>)

## Downloads and public links

For a logged-in ImageKit user, downloads triggered via a public link will appear on the downloads page with the "Via public link" field set to "Yes".

For users visiting a public link, the downloads page will only display downloads triggered from that public link.

![Downloads page public links indicator](<../.gitbook/assets/downloads-page-public-links.png>)
