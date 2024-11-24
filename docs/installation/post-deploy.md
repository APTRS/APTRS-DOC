### First-Time Deployment Checklist for APTRS

Once you deploy APTRS using either the manual method or Docker, follow these steps to ensure everything is set up correctly:

#### 1. Company
Upon the first deployment, APTRS automatically creates a default company named **APTRS PVT**. This company is designated as the internal company, representing the organization using APTRS to generate reports for clients.

- **Key Details:**
    - The internal company cannot be deleted or added via the frontend or API.
    - You can edit its **name**, **address**, and **logo** to match your organization’s details.
    - Any additional companies you create will be treated as client companies.

#### 2. Groups
APTRS includes predefined user groups to facilitate report generation and role management.

- **Default Groups:**
    - **Manager** and **Project Manager** groups are used to include manager details in the reports.
    - It is recommended **not to rename or delete** these groups, as doing so may cause errors during report generation.

#### 3. User and Admin
During the first deployment, APTRS creates a default admin user associated with the internal APTRS company.

- **Key Details:**
    - Do not delete this user, as it is essential for system functionality.
    - You can update the admin user’s **profile details**, such as:
        - **Email address**
        - **Photo**
        - **Name**
        - **Password**
