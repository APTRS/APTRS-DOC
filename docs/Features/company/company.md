# Company Management in APTRS

APTRS allows you to add and manage companies, with two types of companies supported: Internal Companies and External (Non-Internal) Companies.

![Comapany Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/company.png)


## Internal Companies
APTRS assumes that your company, the one deploying and using APTRS, is the internal company. When you deploy APTRS for the first time, an internal company is automatically created. This company is assigned to you and cannot be deleted or replaced. However, you can rename it to reflect your actual company name.

Key details about internal companies:

- **Internal Company Setup**: The internal company is auto-created during the initial setup, and no additional internal companies can be added afterward.
- **User Assignment**: All users added through the User Management page will automatically be part of the internal company.
- **Reports**: Users and data from the internal company will be reflected in your reports as staff or internal users.


## External (Non-Internal) Companies
All companies manually added after the initial setup are classified as External Companies. These are typically client companies or third-party organizations.

Key details about external companies:

- **User Management**: You cannot add users to external companies through the User Management page. Any user added through this page will be assigned as a staff member of the internal company.
- **Assigning Users to External Companies**: To assign users to external companies, you need to go through the Customer section, which manages external users.
- **Projects**: You can create projects for both internal and external companies, allowing you to track and manage work for client companies separately from your own.

This setup ensures a clear distinction between your internal users and external client companies, making it easier to manage access, projects, and reports within APTRS.


!!! warning

    Deleting a company will also delete all associated users. Make sure to reassign or handle any users before deleting a company.

