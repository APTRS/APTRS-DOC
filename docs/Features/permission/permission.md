# Groups and Permissions in APTRS

APTRS implements a robust role-based access control system that uses permissions and groups to manage user capabilities. This system ensures that users have appropriate access to features based on their responsibilities while maintaining system security.

## Permission Management Overview

The permission system in APTRS follows these key principles:

- **Permissions** define specific capabilities within the system
- **Groups** bundle permissions together for easier assignment
- **Users** are assigned to one or more groups
- **Inheritance** ensures users receive all permissions from all their groups

![Group Management Interface](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/group.png)

## Access Control Architecture

### How Permissions Work

Permissions in APTRS are predefined and hard-coded in the backend. Each permission controls access to specific features or APIs within the application. Important aspects include:

- Permissions cannot be created or modified through the user interface
- Each permission provides access to a specific set of functionality
- Permissions are granular to allow precise access control
- The system automatically checks permissions before allowing access to protected features

### Group Management

Groups are collections of permissions that can be assigned to users. They simplify the process of permission assignment by bundling related capabilities:

- You can create custom groups through the Groups Management interface
- Each group can be assigned any combination of available permissions
- Groups can be modified at any time to adjust their permission sets
- Users can be assigned to multiple groups to combine permission sets

!!! tip "Best Practice"
    Create groups based on job functions rather than individuals. This approach makes it easier to maintain consistent permissions as your team changes.

### Admin Users and Special Privileges

Admin users in APTRS have special status within the permission system:

- Admin users have **unrestricted access** to all system features
- Admin status overrides any group-based permission restrictions
- Even with no assigned groups, admin users retain full system access
- Admin privileges cannot be limited through group assignments

!!! warning "Admin Access"
    Admin access should be granted sparingly and only to highly trusted users who require full system access. For most users, it's better to use appropriately configured permission groups.

## Default Groups and Special Considerations

When APTRS is first deployed, the system automatically creates several default groups with predefined permission sets. These groups serve as starting points for your permission structure.

### Default Groups List

| Group Name | Primary Purpose | Key Permissions |
|------------|----------------|----------------|
| Project Manager | Manages projects and appears in reports | Manage Projects, Assign Projects |
| Manager | Oversees operations and appears in reports | Manage Projects, Assign Projects, Manage Users |
| Administrator | Complete system administration | All permissions (similar to admin users) |
| Penetration Tester | Conducts assessments and adds findings | Manage Projects (limited) |

### Report-Critical Groups

!!! important "Special Group Names"
    The **Project Manager** and **Manager** groups are used by APTRS to identify users whose information should appear in generated reports. While you can modify the permissions assigned to these groups, **do not rename them** if you want report generation to function correctly.

If these groups are renamed or removed:

- Reports may not include required management information
- Report templates may display errors or incomplete information
- Report quality and compliance could be affected

## System Permissions Reference

APTRS includes the following core permissions that can be assigned to groups (as of version 2.0):

### Manage Users
- **Capability**: Comprehensive user management
- **Functions**:

    - Add, edit, and delete user accounts
    - Create and manage groups
    - Assign permissions to groups
    - Manage user-group assignments

### Manage Projects
- **Capability**: Full project lifecycle management
- **Functions**:
    - Create, edit, and delete projects
    - Add vulnerabilities to projects
    - Create and manage retests
    - Generate and export reports
    - Define project scope
    - **Note**: By default, users are automatically set as owners of projects they create

### Assign Projects
- **Capability**: Project ownership management
- **Functions**:
    - Select and assign project owners
    - Reassign projects to different users
    - Assign retest ownership
    - Override automatic owner assignment

### Manage Vulnerability Data
- **Capability**: Vulnerability database administration
- **Functions**:
    - Add, edit, and delete vulnerability templates
    - Manage vulnerability categories
    - Maintain finding descriptions and remediation advice

### Manage Customers
- **Capability**: External user management
- **Functions**:

    - Create and manage customer accounts
    - Send and manage customer invitations
    - Assign customers to companies
    - Reset customer credentials when needed

### Manage Companies
- **Capability**: Company profile administration
- **Functions**:
    - Create and edit company profiles
    - Upload and manage company logos
    - Configure company-specific report settings
    - Manage company-project associations

### Manage Configurations
- **Capability**: System configuration
- **Functions**:
    - Adjust system-wide settings
    - Set default behaviors and preferences

## Best Practices for Permission Management

### Creating an Effective Permission Structure

1. **Start with Default Groups**: Use the provided groups as templates
2. **Create Functional Groups**: Align groups with job functions or departments
3. **Follow Least Privilege**: Grant only the permissions necessary for each role
4. **Use Descriptive Names**: Name groups clearly to reflect their purpose
5. **Document Your Structure**: Maintain documentation of your permission scheme

### Security Considerations

- Regularly audit user permissions and group assignments
- Promptly remove permissions when users change roles
- Consider creating temporary groups for contractors or temporary staff
- Review admin user assignments periodically

!!! danger "Permission Drift"
    Without regular auditing, permissions tend to accumulate over time as users are added to groups but rarely removed. This "permission drift" can create security vulnerabilities as users retain access they no longer need.