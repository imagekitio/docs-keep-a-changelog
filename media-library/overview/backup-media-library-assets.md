# Backup media library assets

You can backup all media library assets in your S3 bucket by setting up an active backup.

Go to Settings ➡ Backup bucket - [https://imagekit.io/dashboard\#settings-backup-bucket](https://imagekit.io/dashboard#settings-backup-bucket).

![](../../.gitbook/assets/image%20%2854%29.png)

Here you have to enter the S3 bucket credentials with write permission. 

Learn how to grant write access to an S3 bucket - [https://aws.amazon.com/blogs/security/writing-iam-policies-how-to-grant-access-to-an-amazon-s3-bucket/](https://aws.amazon.com/blogs/security/writing-iam-policies-how-to-grant-access-to-an-amazon-s3-bucket/)

Once the settings are saved, all newly uploaded files will also be uploaded to your backup bucket.

### How do I restore previously uploaded files in the media library?

All newly uploaded files are automatically uploaded to your backup bucket. For old files, please raise a support ticket at support@imagekit.io. Based on the number of objects, additional charges may apply.

### What happens if you overwrite an existing file?

ImageKit will overwrite the file in your backup bucket at the same location.

### What happens when you delete a file or folder in the media library?

ImageKit will not delete the file or folder from the backup bucket.

### What happens if you copy/move a file or folder in the media library?

ImageKit will not copy or move the files or folder in your backup bucket.

