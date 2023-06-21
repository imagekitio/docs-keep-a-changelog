# Draft assets

{% hint style="info" %}
**Enterprise plan only**
This feature is only available in custom enterprise pricing plans.
{% endhint %}

Draft or unpublished assets are files and file versions that can only be accessed via the media library, and their public URLs are not accessible outside of the media library. This feature is useful when you want to upload files to ImageKit but do not want to make them publicly accessible. For example, you can upload files in a draft state and then use them in your application only when you are ready to publish them.

ImageKit allows you to upload files in draft or unpublished states. You can also change the state of any file and its versions that are already present in the media library to draft. You can check the published or draft status of a file on the file detail page.

![Published or draft status of a file (detail page)](<../../.gitbook/assets/published-draft-status.png>)

![Published or draft status of a file (list view)](<../../.gitbook/assets/published-draft-status-list.png>)

## Uploading images in draft state by default

Files are uploaded in published state by default. If you need to ensure that all files uploaded to ImageKit are in the draft or unpublished state, you can configure that option in the media library [settings](https://imagekit.io/dashboard/settings/media-library).

![Upload files in draft state by default](<../../.gitbook/assets/publish-file-on-upload-setting.png>)

## Publish or unpublish a file

You can change the state of a file from draft to published from the media library. Select a file and open the 'Publish or unpublish file' popup. You can check the option to publish or unpublish a file here. If you want this change to apply to all versions of the file, check the 'Include file versions' option.

## Publish or unpublish a multiple files

{% hint style="info" %}
**Limits**
You can only publish or unpublish at most 100 files at once.
{% endhint %}

Similar to a single file, you can publish or unpublish multiple files at once. Select multiple files and open the 'Publish or unpublish file' popup. You can check the option to publish or unpublish a file here. If you want this change to apply to all versions of the files, check the 'Include file versions' option. Unlike a single file, this operation happens asynchronously.

![Publish or unpublish a files](<../../.gitbook/assets/publish-unpublish-file-modal.png>)

## Search for unpublished files

You can search for unpublished files in the media library. Go to the media library and click on the search bar. In the advanced search filters, select 'File published' and select the option to show only unpublished files.

![Search for unpublished files in the media library](<../../.gitbook/assets/search-unpublished-files.png>)

### Understanding behaviour of URLs on unpublishing a file

When you unpublish a file, the public URL of the file and its versions, if they were also unpublished, are no longer accessible. Moreover, a purge cache request is issued for the URL of each file to be unpublished, and it is counted against your account's purge cache quota.

Even if you unpublish only the current version of the file, the cache for all its other versions will also be purged. However, the cache of files transformations made using path parameters would persist, and all transformations having query parameters would be purged.

Note that a file version's URL would change once its state changed from draft to published or vice versa.
