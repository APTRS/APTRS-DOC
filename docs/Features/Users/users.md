# User Management in APTRS

APTRS features a comprehensive user management system that enables secure access control and efficient team collaboration. The system allows administrators to create, modify, and manage user accounts while providing each user with personalized access based on their role and responsibilities.

![User Management Interface](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/user.png)

## User Account Features

- **Authentication**: Secure login with username/email and password
- **Profile Management**: Users can update their profile information and upload profile photos
- **Password Management**: Self-service password changes and reset capabilities
- **Activity Tracking**: System logs user activities for audit purposes

## User Types and Access Levels

APTRS implements a tiered access system to ensure proper security and functionality:

### Administrative Roles

**Admin Users**
- Full system access and configuration rights
- User account management (creation, modification, deactivation)
- Permission and group assignment
- System-wide settings and configuration
- Access to all projects and reports

**Non-Admin Users**
- Access based on assigned permissions and groups
- Limited to specific functions based on role
- Can be granted selective administrative capabilities

!!! info "Permission Management"
    For detailed information on managing permissions and security groups, see the [Permissions and Groups](../permission/permission.md) documentation.

### User Classification

Users in APTRS are categorized based on their relationship to your organization:

**Internal Users (Staff)**
- Your organization's team members
- Default classification for newly created users
- Full access to internal systems based on their role
- Can be assigned as project owners and team members

**External Users (Non-Staff)**
- Client representatives and external collaborators
- Limited access to specific projects and reports
- Customizable permission sets for client needs
- Typically managed through the [Customer Portal](../company/customer.md)

## User Management Best Practices

### Adding New Users

1. Navigate to the Users section in the administrative interface
2. Select "Add User" and complete the required fields
3. Assign appropriate groups and permissions
4. Set initial password or trigger email invitation

### Modifying User Access

User accounts can be modified at any time to:
- Update contact information
- Change role assignments
- Adjust permission levels
- Enable/disable specific features

### User Deactivation vs. Deletion

!!! warning "Important: Project Ownership"
    Every project in APTRS must have an assigned owner. Deleting a user account that owns projects will cause system errors.

**Recommended Approach**: Instead of deleting user accounts, set them to "inactive" status when:
- An employee leaves your organization
- A client relationship ends
- A temporary user no longer needs access

**If Deletion is Necessary**:

- Identify all projects owned by the user
- Reassign each project to a new owner
- Remove the user from all groups and permission sets
- Delete the account only after confirming all dependencies are resolved

## User Security Features

APTRS implements several security measures for user accounts:

- Password complexity requirements
- Failed login attempt monitoring
- Session timeout controls
- IP-based access restrictions (optional)
- Two-factor authentication support (available in select versions)

By properly managing user accounts, you can ensure secure, efficient operation of your APTRS implementation while maintaining appropriate access controls for your penetration testing reports and client data.