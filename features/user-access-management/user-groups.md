# User groups

{% hint style="info" %}
**Enterprise plan only**
This feature is only available in custom enterprise pricing plans.
{% endhint %}

The Digital Asset Management workflow in ImageKit offers several options for controlling user access and permissions on files, folders, and media collections. As per your needs, you may want to define different sets of users that need access to which resources and what permission level they require based on the actions they need to perform. You can establish a user's access and permission level across the files, folders, and media collections when you assign them the "Media library restricted user" role. When using user groups, it is easier to control access and permissions to shared resources all at once rather than configuring them separately for each restricted user.

## How to view and manage user groups?

Go to the User management section in ImageKit's dashboard. Under the User groups tab in the side menu, you will find all the options to view and manage your user groups.

{% hint style="info" %}
**User group limits**
- There are no limits to the number of users that can be added to a user group.
- There are no limits to the number of user groups that can be created.
- A user group's name is unique and case-insensitive. They can only contain alphabets, numbers, and spaces.
{% endhint %}

## How it works?

Once you create a user group, any user with complete access to the media library or any restricted user with "Manage" permission on the desired files, folders, and media collections can share the resource with the user group. While sharing, they also have to configure the [permission level]() of the resource that they want to provide to the user group. Once done, all the selected users will have access with the configured permission on the resource through the user group.

Users also have access to all files and folders included in the shared folder and media collection. It should be noted that a user's permission level on any asset is the highest permission level that they may have from being shared on multiple resources. For example, if a user has "Manage" access to a folder via a user group and "Contribute" access via a media collection, the permission they have on the folder is the highest permission level available via all shared methods, which is "Manage" in this case.

When a user group is deleted, users lose access to any files, folders, or media collections that they had access to through the group.

When a user's role is changed from "Restricted media library access" to some other role or when any such user is deleted, they are removed from the user group.
