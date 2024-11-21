
# Groups and Permissions in APTRS

APTRS uses permissions to control access to certain APIs and restrict features to specific users. To streamline this process, APTRS allows you to create groups and assign permissions to those groups. Once a group is created, you can assign users to one or more groups. A user assigned to multiple groups will inherit all the permissions from those groups.

![Group Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/group.png)



### How Permissions Work
Permissions are pre-defined within APTRS to regulate access to various functionalities. These permissions cannot be modified or created from the user interface, as they are hard-coded in the backend. However, you can create custom groups and assign these predefined permissions to the groups. When users are assigned to a group, they will automatically gain all the permissions associated with that group.

### Admin Users and Permissions
Admin users in APTRS are not restricted by permissions or group assignments. This means that even if an admin user has no group assigned or is assigned to a group with limited permissions, they will still have full access to all features and APIs in the system. Admin users are always granted full control, regardless of group membership or assigned permissions.

### Default Groups
When you first deploy APTRS, several default groups are created with specific permissions. The most important groups are Project Manager and Manager. These groups are crucial for report generation, as they are used to add project manager and manager details to reports.

While you can edit the permissions assigned to these groups, it is important to retain the group names ("Project Manager" and "Manager") if you want APTRS to correctly include project manager and manager details in the reports. Removing or renaming these groups may cause issues with report generation and prevent the correct user details from being added to reports.

### List of Permissions (as of version 1.0)

- Manage Users:
Users with this permission can add, edit, and delete users, manage groups, and assign permissions to groups. They can also create and delete groups.

- Manage Projects:
Users with this permission can create, edit, and delete projects. They can add vulnerabilities or retests within a project, generate reports, and add scope to projects. By default, users cannot select project or retest owners; projects and retests created by users with this permission will automatically mark the creator as the owner.

- Assign Projects:
This permission allows users to select and assign project or retest owners. Users with this permission can assign any user as the owner of a project or retest.

- Manage Vulnerability Data:
Users with this permission can add, edit, and delete entries in the vulnerability database or templates.

- Manage Customers:
Users with this permission can add, edit, and delete customers.

- Manage Companies:
Users with this permission can add, edit, and delete companies.

- Manage Configurations:
Users with this permission can manage various application configurations.