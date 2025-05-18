# Post-Deployment Setup

After successfully deploying APTRS through either the manual method or Docker, follow this checklist to ensure your system is properly configured and ready for use.

## First-Time Deployment Checklist

!!! tip "Before You Begin"
    Keep this checklist handy to verify that all critical components are properly set up in your new APTRS installation.

### 1. Company Setup

Upon first deployment, APTRS automatically creates a default company named **APTRS PVT**. This serves as your internal company, representing the organization using APTRS to generate reports for clients.

!!! important "Internal Company Management"
    - The internal company **cannot be deleted or added** via the frontend or API
    - You should edit its **name**, **address**, and **logo** to match your organization's details
    - Any additional companies you create will be treated as client companies

### 2. User Groups

APTRS includes predefined user groups that are essential for proper report generation and role management.

!!! warning "Default Groups"
    - **Manager** and **Project Manager** groups are used to include manager details in the reports
    - Do **not rename or delete** these groups, as doing so may cause errors during report generation

### 3. Admin User

During first deployment, APTRS creates a default admin user associated with your internal company.

!!! caution "Admin Account Security"
    - Do not delete this user, as it is essential for system functionality
    - For security, immediately update the admin user's profile details:
        - **Email address** (use a secure company email)
        - **Password** (use a strong, unique password)
        - **Name** (real name or designated admin title)
        - **Photo** (optional: company logo or admin photo)

## Next Steps

After completing the initial setup, you may want to:

1. Create additional user accounts for your team members
2. Add client companies to your system
3. Configure vulnerability templates
4. Customize report templates to match your brand
5. Review system settings for optimal performance

Refer to the [Features](/Features/index) documentation for detailed information on using these capabilities.
