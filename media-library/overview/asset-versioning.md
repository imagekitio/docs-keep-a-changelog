# Asset versioning

Versioning in ImageKit keeps multiple variants of an asset. It allows you to preserve, retrieve, and restore every version of any asset stored in your media library. As a result, you can recover more quickly from both unintended user actions and application failures with versioning.

If ImageKit receives multiple upload requests for the same asset with the same name and location, it creates a new version.

{% hint style="info" %}
Versioning is enabled on all accounts on any pricing plan. All current plus previous versions of an asset count towards your total storage usage.
{% endhint %}

The following actions can be performed from the dashboard and programmatically via the APIs:

1. Create a new file version by uploading a file with the same name and location as an existing file. This can be done by using upload API or using [Remove.bg extension](../../extensions/background-removal.md) which uploads a new file with background removed.
2. Restore the file to a previous version.
3. Delete any non-current file version.
4. Get any file version's detail.
5. Get details of all versions of a file.
6. Search for file versions.

{% hint style="info" %}
**Version limits**\
A asset can have a maximum of 100 versions.
{% endhint %}

## Searching for file versions in the media library

Go to your [media library](https://www.youtube.com/watch?v=WyOWZ50p-Vc&list=PLHMsBwDlzJRy9Y3xPpHZWjx_aRW5IOQQd&index=10&t=2s) in ImageKit's dashboard and click on the search bar. Check the option to include file versions in search results to get the desired outcome. You can identify if an asset is a file version by looking at a badge marking it.

Check out the video below to learn how to include file versions in search results.

{% embed url="https://www.youtube.com/watch?v=qjDKpSYCW7U" %}

{% hint style="info" %}
**Viewing versions**\
Non-current file versions do not appear in the media library by default. You have to search for file versions or navigate to the details page of an asset to view its different versions.
{% endhint %}

## Media library operations

The "Restore version" and "Delete version" options are available in the context menu when a file version is selected. Using these options, you can restore the selected file version to the current version of the file or delete the file version respectively. These actions are also available in the menu of the asset details page of file versions. Note that these operations are only available for the non-current version of a file.

![File version context menu options](<../../.gitbook/assets/version-context-menu.png>)

### Copy/move file

When a file is copied/moved to a destination that has an existing file of the same name, the source file versions are appended to the destination file version's history.

In a copy operation, an option is available to copy all file versions in the folder selection drawer. If it is not checked, which is the default behavior, only the current file version is copied. Whereas, in the move operation all source file versions are moved to the destination.

![Include file versions in copy operation](<../../.gitbook/assets/copy-include-versions.png>)

{% hint style="info" %}
**File and file version operations**\
Media library operations available for files (current version) and file versions (non-current versions) are different. For example, you cannot update tags, copy, move, rename, edit, download/download (as zip), use extensions, etc., on non-current versions of a file. Read ImageKit's [media API reference](../api-reference/media-api) for more details.
{% endhint %}

## Details page

You can choose the version you want to view on the asset details page by clicking the dropdown at the top and selecting the desired version.

![Non-current file version details page](<../../.gitbook/assets/non-current-version-details.png>)

![Current/latest file version details page](<../../.gitbook/assets/current-version-details.png>)

## URLs and transformations

You can access and transform any version by using its unique URL and ImageKit transformation parameters.
