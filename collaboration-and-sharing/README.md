# Collaboration and sharing

ImageKit's Digital Asset Management solution allows you to organize, manage and share your assets in files and folders. Using media collections, you can create virtual groupings of assets regardless of their folder structure, providing a simple way for your users to collaborate on those assets.

ImageKit allows you to set access and permission levels for each user on your assets as per your organization's needs. Based on the workflows that are essential for your business, you can decide which assets a user can see and what actions they can perform on them.

Based on your needs, you can create your DAM workflow and organize, share, and control access for users and user groups with various permission levels.

## Access and permission management

{% hint style="info" %}
A new restricted media library user will see an empty media library until any files, folders, or media collections are shared with them.
{% endhint %}

ImageKit allows you to control access to assets by sharing media collections, files, and folders (and therefore their subfolders and files) with selected restricted media library users or user groups at varied permission levels ranging from view-only to full administrative or manage access. Implicitly, by not sharing an asset or media collection with users or user groups, you prevent access to those resources and their contents by such users.

It is the responsibility of the account administrator to set up the file and folder hierarchy as well as assign entities the proper initial permission levels across users and user groups. Remember that users may be able to share files, folders, and media collections with other user groups or particular users with varied levels of access permission based on the permission levels you've provided. This implies that more users would have access to resources that you initially did not offer them. Also, their permission level can permit them to delete resources or carry out other such destructive actions on them.

Users with the following roles or permissions can share assets and media collections with restricted media library users and user groups:

1. Account administrator
2. Developer
3. Media library full access
4. Media library restricted access with "Can manage" permission on an asset

## How to share assets and media collections?

You can share files, folders and media colletions with other restricted media library users and user groups.

- [Share files and folders](./share-files-and-folders.md)
- [Share media collection](./share-media-collection.md)

To share an asset or media collection, click the "Share" button in the dropdown menu after selecting it. A popup would appear, allowing you to pick the users and user groups, as well as specify their access level on the entity under the "User & user groups" tab. You may also view, change or remove the permissions of existing users and user groups that have access to the entity.

You can also see the media collections to which the selected asset has been added by clicking on the "Media collection associations" tab in this popup.

## Understanding access to assets and media collections

{% hint style="info" %}
Access control and permissions can only be managed for restricted media library users.
{% endhint %}

They can only perform actions on files and folders that are shared with them based on their permission levels. However, such users can create new media collections.

An asset or media collection can be shared with users in the following ways:

1. **Sharing with users**
   A user can be given access to any asset or media collection individually. This is useful when you want to give users access to certain resources with different permission levels that you may not need to share with other users.

2. **Sharing with user groups**
   The majority of your creative or marketing team will probably have similar workflow requirements and access and permission needs. You can streamline your workflow by defining asset or media collection access permissions for user groups, which can be associated with many users, as opposed to defining those permissions for one user at a time.

3. **By asset or media collection ownership**
   A user who creates a file, folder, or media collection is the owner of that resource, and they have "Can manage" permission on it.

   For example, if a user creates a folder, they can perform all operations that "Can manage" permission level allows on the folder and all the assets inside that folder. Even if you revoke the access through which they were able to create the folder in that directory, they would still have access with that permission level to that particular asset and any subfolders and files inside it.

### Understanding how a user's role change affects access

When a user's role is changed from "Restricted media library access" to some other role or when any such user is deleted, they are removed from the user group. If their role is changed back to "Restricted media library access", they would still have access to all the assets and media collections that were previously individually shared with them. However, they won't be added back to any of the user groups they were previously added to.

### Understanding how deleting user groups affects access

When a user group is deleted, users lose access to any files, folders, or media collections that they had access to through the group unless it was shared with them directly.

## Recommended workflow

It is important to plan out your DAM workflow carefully. Your media collections, folders, and files should be organised such that your team members have appropriate access and permissions to the right resources.

To help you establish your workflow, we recommend the following steps:

1. Define your teams.
   Consider the various teams in your organization. Do you, for example, have a design, marketing, sales, and technology team? This lets you think in terms of different teams and what sort of permission and access they will need in the next step. Once you have determined that, you can add them to their respective user groups.

2. Organize your assets and media collections.
   A very important aspect of setting up your workflow is creating your folder structure. Keep in mind that permissions would cascade down to its subfolders, meaning that if you grant a certain level of access to a folder, you can't restrict that level of access from any of its sub-folders or files.

   Create separate folders for each team (or user) in the root media library and grant appropriate permissions. Avoid deep nesting of folders, as this will increase the chances of permission escalation by mistakenly giving higher permission to some parent folder.

   You can also add your assets to media collections for users to view, and if required permissions are set for restricted users, contribute to or manage assets across different locations centrally and share them with relevant stakeholders.

3. Share assets and media collections with your team members.
   For each user group that you want to share it with, grant the appropriate level of access.

   For example, the marketing team will need sufficient permission to add, remove, and modify assets in certain folders. Technology or the sales team might only need read/view permission for specific folders containing brand assets. As a content manager, you may want to ensure all communications use the latest brand assets that are managed and kept up-to-date centrally by a marketing team.

   If you create a new restricted media library user before sharing any assets or media collections with them, the media library will appear completely empty. You can share a folder with at least "Can contribute" permission on it so that they can start uploading files.

4. Invite users and add them to user groups
   Invite users with "Restricted media library access" into your account. Add them to the respective user groups based on your requirements and their roles in your organization.

   Grant permissions to individual restricted users if you have users who need permissions to assets and media collections that their groups don't have.

## Permissions

### Understanding permissions on assets and media collections

When you share a folder, its permission level cascades down to all its subfolders and files.

The permission level of a user or user group can be increased in a subfolder of a folder that they already have access to, but it cannot be lowered.

If you don't share a file and folder (or any parent of that folder) with a particular user or group by any means, those users will not be able to see that folder or the contents inside it. Even when performing a search on all folders, the results will only include folders where the user has at least view permission. Furthermore, if you don't share any folders with a particular user or user group, then those users won't have access to any assets in the Media Library.

If assets are added to a media collection, and that media collection is shared with a user or user group who otherwise do not have access to that asset, those users will still be able to take all "Can view" permission actions on the assets in that media collection. However, they won't be able to modify those assets unless they have "Can contribute" or "Can manage" permission on them.

An asset can be shared with a user in different ways and with varying permission levels. The permission level on the asset would be the highest permission the user derives through all these means. For example, if a user is in multiple groups and the same folder is shared with each of those groups at different permission levels, then the highest of those permission levels applies to the user for that asset.

{% hint style="info" %}
You cannot share the Home (root) of the media library with any restricted media library user.
{% endhint %}

### File and folder permission levels

An ImageKit account user with relevant permissions can view and manage access permissions on an asset (files and folders) for restricted media library users and user groups. When a permission is set on a folder, it applies to all of its subfolders and files. For an asset, you can assign one of the following permission levels:

1. Can view
2. Can contribute
3. Can manage

The table below summarises the operations that can be performed on media library assets at each permission level:

| Operation                        | Can view           | Can contribute     | Can manage         |
| -------------------------------- | ------------------ | ------------------ | ------------------ |
| View & search assets             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View file & file version details | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Download file                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Download assets as zip           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View downloads                   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View comments                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Edit and delete comments         |                    | :heavy_check_mark: | :heavy_check_mark: |
| Create new folder                |                    | :heavy_check_mark: | :heavy_check_mark: |
| Upload file & new file version   |                    | :heavy_check_mark: | :heavy_check_mark: |
| Restore file version             |                    | :heavy_check_mark: | :heavy_check_mark: |
| Update custom metadata           |                    | :heavy_check_mark: | :heavy_check_mark: |
| Edit tags                        |                    | :heavy_check_mark: | :heavy_check_mark: |
| Add auto tags (extension)        |                    | :heavy_check_mark: | :heavy_check_mark: |
| Remove background (extension)    |                    | :heavy_check_mark: | :heavy_check_mark: |
| Remove tags                      |                    | :heavy_check_mark: | :heavy_check_mark: |
| Edit image                       |                    | :heavy_check_mark: | :heavy_check_mark: |
| Edit custom focus area           |                    | :heavy_check_mark: | :heavy_check_mark: |
| Copy asset                       |                    | :heavy_check_mark: | :heavy_check_mark: |
| Add asset to media collections   |                    | :heavy_check_mark: | :heavy_check_mark: |
| Rename file                      |                    |                    | :heavy_check_mark: |
| Move asset                       |                    |                    | :heavy_check_mark: |
| Delete file version              |                    |                    | :heavy_check_mark: |
| Delete asset                     |                    |                    | :heavy_check_mark: |
| Share asset                      |                    |                    | :heavy_check_mark: |

### Media collection permission levels

An ImageKit account user with relevant permissions can view and manage access permissions on media collections for restricted media library users and user groups. For a media collection, you can assign one of the following permission levels:

1. Can view
2. Can contribute
3. Can manage

The table below summarises the operations that can be performed on media collections and assets added to them at each permission level:

| Operation                           | No permission      | Can view           | Can contribute     | Can manage         |
| ----------------------------------- | ------------------ | ------------------ | ------------------ | ------------------ |
| Create new media collection         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View & search media collections     |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View assets in media collection     |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| View file & file version details    |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Download media collections as zip   |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Add assets to media collection      |                    |                    | :heavy_check_mark: | :heavy_check_mark: |
| Rename media collection             |                    |                    |                    | :heavy_check_mark: |
| Remove assets from media collection |                    |                    |                    | :heavy_check_mark: |
| Delete media collection             |                    |                    |                    | :heavy_check_mark: |
| Share media collection              |                    |                    |                    | :heavy_check_mark: |
