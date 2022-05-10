# Asset versioning

ImageKit's media library allows you to maintain and manage multiple versions of an asset. These actions can be performed from the dashboard and programatically via the APIs. 

You can-
1. Create a new file version by uploading a file with the same name and location as an existing file. Using [Remove.bg extension](../../extensions/background-removal.md) creates a new version of the file.
2. Restore file to any of its non-current version.
3. Delete any non-current file version.
4. Get any file version's detail.
5. Get details of all versions of a file.
6. Search for file versions.

{% hint style="info" %}
**Version limits**\
A asset can have a maximum of 100 versions.
{% endhint %}

## Searching for file versions in media library

Go to your [media library](https://imagekit.io/dashboard/media-library) in ImageKit's dashboard and click on the search bar. Check the option to include file versions in search results to get the desired outcome. You can identify if an asset is a file version by looking at a badge marking it as such.

![Include file versions in search results](<../../.gitbook/assets/file-version-search.mov>)

{% hint style="info" %}
**Viewing versions**\
Non-current file versions do not appear in the media library by default. You have to search for file versions or navigate to the details page of an asset to view its different versions.
{% endhint %}

## Media library operations

When a file version is selected, the "Restore version" and "Delete version" options are available in the context menu. Using these options you can restore the selected file version to current version of the file, or delete the file version respectively. These actions are also available in the menu of the asset details page of file versions. Note that these operations are only available for non-current version of a file.

![File version context menu options](<../../.gitbook/assets/version-context-menu.png>)

### Copy/move file

When a file is copied/moved to a destination which has an existing file of same name, the source file versions are appending to the destination file version's history.

In copy operation, an option is available to copy all file versions in the folder selection drawer. If it is not checked, which is the default behaviour, only the current file version is copied. Whereas, in move operation all source file versions are moved to the destination.

![Include file versions in copy operation](<../../.gitbook/assets/copy-include-versions.png>)

{% hint style="info" %}
**File and file version operations**\
Media library operations available for files (current version) and file versions (non-current versions) are different. For example you cannot update tags, copy, move, rename, edit, download/download (as zip), use extensions etc. on non-current versions of file. Read ImageKit's [media API reference](../api-reference/media-api) for more details.
{% endhint %}

## Details page

In the asset details page you can choose the version you want to view by clicking the dropdown at the top and selecting the desired version. You can also perform the available operations for file or file version from the details page.

![Non-current file version details page](<../../.gitbook/assets/non-current-version-details.png>)

![Current/latest file version details page](<../../.gitbook/assets/current-version-details.png>)

## URLs and transformations

You can use URLs of all versions of an asset as well as apply transformations on them.