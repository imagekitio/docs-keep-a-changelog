# Upload files

Currently, you can upload images and static files to the media library in two ways - using the dashboard, and using the [upload API](../../api-reference/upload-file-api/). ImageKit.io currently supports [these formats for upload](../../api-reference/upload-file-api/#allowed-mime-types-for-uploading).

{% hint style="info" %}
**File size limit**  
The maximum upload file size is limited to 25MB.
{% endhint %}

## Using Dashboard

Within the dashboard, navigate to the 'Media Library' section.

To upload files, either drag and drop files anywhere on the screen or

### Select the file and upload

 Click on the New button on the top right corner and select File Upload.

![](../../.gitbook/assets/new-file-upload.png)

Select the file from the local device to start uploading.

{% embed url="https://youtu.be/IO-PJKU5X7E" caption="Select file and upload" %}

### Drag & drop file upload

You can drag multiple files anywhere on the media library screen to upload.

{% embed url="https://youtu.be/W5RcugTXPks" caption="Drag and drop file upload" %}

{% hint style="info" %}
**Info:** When uploading images, ImageKit.io appends a random string to the image file name to avoid replacing images with the same file name. You can turn off this setting within the Upload Section of [Image Settings](https://imagekit.io/dashboard#settings) within the dashboard.
{% endhint %}

## Using the Upload API

You can also upload images to ImageKit.io's media library programmatically. Read the Upload API documentation [here](../../api-reference/upload-file-api/).

