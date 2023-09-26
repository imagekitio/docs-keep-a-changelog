# Draft assets

{% hint style="info" %}
**Enterprise plan only**
This feature is only available in custom enterprise pricing plans.
{% endhint %}

Draft or unpublished assets are files and file versions that can only be accessed via the media library, and their public URLs are not accessible outside of the media library. This feature is useful when you want to upload files to ImageKit but do not want to make them publicly accessible. For example, you can upload files in a draft state and then use them in your application only when you are ready to publish them.

ImageKit allows you to upload files in draft or unpublished states. You can also change the state of any file and its versions that are already present in the media library to draft. You can check the published or draft status of a file on its detail page.

![Published or draft status of a file (detail page)](<../../.gitbook/assets/published-draft-status.png>)

![Published or draft status of a file (list view)](<../../.gitbook/assets/published-draft-status-list.png>)

## Uploading images as drafts by default

By default, files are uploaded as published. If you want to ensure that all files uploaded to ImageKit are in the draft or unpublished state, you can configure that option in the media library [settings](https://imagekit.io/dashboard/settings/media-library).

![Upload files in draft state by default](<../../.gitbook/assets/publish-file-on-upload-setting.png>)

## Publish or unpublish files

{% hint style="info" %}
**Limits**
You can only publish or unpublish at most 100 files at once.
{% endhint %}

You can publish or unpublish a single or multiple files at once. Select the files and open the 'Publish or unpublish file' popup. You can check the option to publish or unpublish a file here. If you want this change to apply to all versions of the files, check the 'Include file versions' option. This operation happens asynchronously.

![Publish or unpublish a files](<../../.gitbook/assets/publish-unpublish-file-modal.png>)

## Search for draft files

You can search for files and file versions in draft state in the media library. Go to the media library and click on the search bar. In the advanced search filters, select 'File published' and choose the option to show only unpublished files.

![Search for unpublished files in the media library](<../../.gitbook/assets/search-unpublished-files.png>)

### Understanding the behaviour of URLs on unpublishing a file

When you unpublish a file, the file's public URL will no longer be accessible. In addition, a purge cache request is triggered for the unpublished file. It's important to note that these purge cache requests count towards the quota allocated to your account.

Even if you decide to unpublish only the current version of the file, the cache for all its other versions will also be cleared. However, the cache for file transformations made using path parameters will persist, while any transformations that include query parameters will be purged.

{% hint style="info" %}
**File version URL**
Publishing or unpublishing a file version changes its URL.
{% endhint %}

When a file version transitions between the draft and published states, its URL will change accordingly. The URL changes because the `ik-obj-version` query parameter changes after the operation, and the old one is no longer valid. A file version's URL looks like this: `https://ik.imagekit.io/your_imagekit_id/file.jpg?ik-obj-version=iAg8gxkqo_QUBwqNeQrJzuyce2XB7Gc4`
