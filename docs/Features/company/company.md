# Company Management in APTRS

APTRS provides a structured company management system that distinguishes between your organization and your clients. This separation is fundamental to how APTRS organizes projects, users, and reports.

!!! tip "Key Benefits of Company Management"
    - **Organization**: Clear separation between internal and client work
    - **Branding**: Customize reports with appropriate company logos
    - **Access Control**: Manage permissions based on company associations
    - **Reporting**: Generate professional reports with correct company attribution

![Company Management Interface](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/company.png)

## Accessing Company Management

The Company Management module can be accessed through:

1. Direct URL navigation to `/companies`

Only administrators and users with company management permissions can access this feature.

## Company Types

APTRS operates with two distinct company types, each serving different purposes within the platform.

### Internal Company

The internal company represents your organization—the security team or consultancy using APTRS to deliver penetration testing services.

**Key Characteristics:**

- **Automatic Creation**: Generated during first-time setup
- **Single Instance**: Only one internal company can exist in the system
- **Customization**: Can be renamed and branded to match your organization
- **Persistence**: Cannot be deleted as it's essential to system operation

**Internal Staff Management:**

- All users created through the User Management interface are automatically assigned to the internal company
- These users appear in reports as members of your organization
- Internal users can be assigned roles like Penetration Tester, Manager, or Report Reviewer

!!! warning "Internal Company Configuration"
    Proper configuration of your internal company is crucial as it defines how your organization appears on all reports and communications. Be sure to:
    - Upload a high-quality company logo (recommended size: 300x100px)
    - Provide accurate company details including address and contact information
    - Set appropriate default email templates

### External Companies

External companies represent your clients or third-party organizations for whom you conduct security assessments.

**Key Characteristics:**

- **Manual Creation**: Added as needed through the Company Management interface
- **Multiple Instances**: No limit to the number of external companies
- **Client Representation**: Each external company can have its own branding, contacts, and projects
- **Deletion**: Can be removed when client relationships end (with important caveats)

**External Company Benefits:**

- Organize client projects under their respective companies
- Generate branded reports with client company details
- Create customer portals specific to each client
- Maintain separate communication threads per client

## Comparison of Company Types

| Feature | Internal Company | External Companies |
|---------|------------------|-------------------|
| **Creation** | Automatic during setup | Manually created as needed |
| **Quantity** | Single instance only | Unlimited |
| **Users** | Internal staff | Client representatives |
| **Deletion** | Not possible | Possible with caution |
| **Projects** | Internal assessments | Client assessments |
| **Primary Role** | Service provider | Service recipient |
| **Report Appearance** | Author organization | Target organization |

## User Association with Companies

APTRS maintains a clear distinction between internal and external users:

| User Type | Company Association | Creation Method | Primary Purpose |
|-----------|---------------------|-----------------|-----------------|
| Internal | Your organization | User Management interface | Conducting assessments, creating reports |
| External | Client companies | Customer section | Accessing delivered reports, communication |

!!! info "External User Management"
    For details on creating and managing external users, see the [Customer Management](customer.md) documentation.

### Managing User-Company Relationships

Users in APTRS are always tied to a specific company:

1. **Internal users** can be assigned to work on any project but remain associated with your internal company
2. **External users** can only access information related to their specific company
3. **User permissions** are influenced by both role and company association
4. **Email notifications** are customized based on company settings

## Projects and Company Association

Each project in APTRS must be associated with a company:

- **Internal Projects**: Assessments conducted for your own organization
- **External Projects**: Client-commissioned security assessments

This association determines how the project is categorized, who can access it, and how it appears in reports and dashboards.




### Company Lifecycle Management

When client relationships change:

- Update company information as needed
- Archive completed projects before considering company deletion
- Export any necessary data for compliance or record-keeping
- Consider deactivating rather than deleting companies with historical data

!!! danger "Deletion Warning"
    **Deleting a company will permanently remove:**
    - All associated customer users and their access
    - Company information from all reports (potentially breaking report formatting)
    - Historical company data

    Always reassign or export critical data before deletion.

## Creating a New External Company

To add a new external company to APTRS:

1. Navigate to the Company Management section
2. Click the "Add Company" button
3. Complete the company profile form with:
   - Company name
   - Address information
   - Company logo (upload)
4. Save the new company profile


## Editing Company Information

Company details can be modified at any time by:

1. Navigating to the Company Management section
2. Locating the company in the list
3. Clicking the "Edit" action
4. Updating the necessary information
5. Saving changes

All changes to company information will be reflected in new reports, but won't automatically update previously generated documents.

