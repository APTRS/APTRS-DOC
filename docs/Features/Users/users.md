# User Management in APTRS

APTRS provides robust user management capabilities, allowing you to add, edit, and delete users. Each user can log in to the APTRS platform using their credentials, and they have the option to set a profile photo through the Edit Profile feature. In version 1.0, users are also able to change their passwords.


![User Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/user.png)

### User Types
There are two types of users in APTRS:

1. Admin Users: Admin users have elevated permissions and can perform administrative tasks, including managing user accounts and configurations.
2. Non-Admin Users: Non-admin users have restricted access and can perform tasks based on the permissions and groups assigned to them.


All users can log in and perform tasks according to their assigned permissions and groups. 


!!! info 

    For detailed information on permissions and groups, please refer to the Permissions and Groups section.


### User Classification
Users are classified into two categories:

1. Internal Users (Staff Users): By default, all users added to the system are marked as staff users, indicating they are internal users of the APTRS.
2. External Users (Non-Staff Users): External users are designated as non-staff users. For details on managing non-staff users, please refer to the Customer section.

This structured approach to user management ensures that you can effectively manage access levels and user capabilities within the APTRS platform.


### Important Note on Deleting Users
All projects in APTRS must have an owner. Deleting a user will cause errors, as projects assigned to that user will no longer have an owner. If you need to disable a user’s account, it is recommended to set the account to "inactive" rather than deleting it. If you choose to delete the user, be aware that you will need to manually reassign ownership of all the projects associated with that user to avoid issues.