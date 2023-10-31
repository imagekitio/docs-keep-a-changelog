# Public shareable links

{% hint style="info" %}
**Enterprise plan only**
This feature is only available in custom enterprise pricing plans.
{% endhint %}

You can share assets and media collections with users who are not part of your ImageKit account using public links. Public links allow limited view-only access to your assets and media collections, i.e., users accessing them via public links will not be able to perform edit or delete operations on them. Moreover, you have the option to password-protect and set expiration on the public links.

## Understanding access to assets and media collections via public links

When accessing an asset or media collection using a public link, a user can-
- View file metadata like dimensions, size, and embedded metadata. However, they won't be able to see details like tags, AI tags, custom coordinates, custom metadata, ownership details, or comments on that file.
- Download assets and media collections.
- View private files. However, whether a file is private or not is not visible in this view.
- View draft files inside shared folders and media collections, if exposed using the "Expose unpublished files" checkbox. However, whether a file is published or not is not visible in this view. This can only be done if you have the [Draft assets](../media-library/overview/draft-assets.md) feature enabled.
- View only the current version of the file. Any non-current version of a file will not be accessible via the public link.

Note that when an asset or media collection is deleted or moved, its public link is also deleted.

![Public links view](<../.gitbook/assets/public-links-view.png>)

{% hint style="info" %}
**Limits**
- For a single asset or media collection, a maximum of 5 public links can be created.
- Across all assets and media collections in an account, a maximum of 100 public links can be created. This limit is configurable.
{% endhint %}

## Creating and managing public links for assets and media collections

Select the asset or media collection that you want to share and right-click on it. Select the "Public links" option from the menu. Here you will get the option to create or manage any existing public link configurations on it.

![Right-click menu location for public links](<../.gitbook/assets/right-click-menu-public-links.png>)

![Form for creating a new public link](<../.gitbook/assets/new-public-link-form.png>)

You can configure the following options for a public link:
- Validity timeframe: You can set the "Validity start" and/or "Validity end" dates for any public link. At any time that is either earlier than the "Validity start" time or later than the "Validity end" time, the link will not work. After a link has expired, it will be automatically deleted.
- Password protection: If a password is set for a public link, any user trying to access the link will be prompted to enter that password before access is granted. This password can be changed at any time without affecting the URL of the public link.
- Expose unpublished assets: When creating a public link for a folder or media collection, this setting defines whether any unpublished assets that fall under that folder or media collection can be accessed via the link or not. If a public link is created for a single file, then the file will be accessible via the link, regardless of its publication status. Note that this option is only available if you have this feature enabled.

### Managing all public links from media library settings

You can view, edit, and delete public links for all assets and media collections in the media library [settings page](https://imagekit.io/dashboard/settings/media-library). Media library restricted users do not have access to this page.

![Table displaying all public links in an account](<../.gitbook/assets/all-public-links-table-settings-page.png>)

## Download as a zip in public links

For a logged-in ImageKit user, downloads triggered via a public link will appear on the downloads page with the "Via public link" field set to "Yes".

For users accessing a public link, the downloads page will only display downloads triggered by that public link for their currently active session.

![Downloads page public links indicator](<../.gitbook/assets/downloads-page-public-links.png>)

## Central audit logs

If [Central audit logs](../features/central-audit-logs.md) is included in your pricing plan, you can view logs of all create, update, and delete operations of public links.

## Access control and permissions

A restricted media library user needs to have at least "manage" permission on an asset or media collection to be able to view, create, edit, or delete its public links. Learn about actions that can be performed on media collections and assets by restricted media library users [here](../collaboration-and-sharing/README.md#understanding-permissions-on-assets-and-media-collections).
