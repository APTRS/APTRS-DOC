# Configuration

The Configuration section allows admin users or users with the Manage Configurations permission to add and manage various settings. In APTRS, users can define Project Types, such as "Mobile Application Testing," which are used when creating projects and are also referenced in PDF and DOCX reports. Multiple project types can be added to suit different testing needs.

In addition to project types, users can also create Report Standards. When generating a report, users are required to select the applicable testing standard, such as OWASP for web application testing. Users can add additional standards here, which can then be used in the generated reports.

!!! info "Version 2.0 Update"
    As of APTRS version 2.0, users can now edit and delete existing project types and report standards. This provides more flexibility in managing your configuration settings and keeping them up-to-date.

![Configuration Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/config.png)

## Managing Configuration Settings

The Configuration section in APTRS provides tools to customize the system according to your organization's needs. This section explains how to manage these configuration options.

### Project Types

Project Types define the categories of security assessments your organization performs. These appear in project creation forms and are included in generated reports.

#### Adding Project Types

To add a new project type:

1. Navigate to the Configuration page
2. Locate the Project Types section
3. Click the "Add Project Type" button
4. Enter the name of the project type (e.g., "Web Application Assessment", "API Security Testing")
5. Click "Save" to add the project type

#### Editing Project Types (Version 2.0+)

To modify an existing project type:

1. Find the project type in the list
2. Click the "Edit" icon next to the project type
3. Update the name as needed
4. Click "Save" to apply your changes

!!! warning "Impact on Existing Projects"
    Editing a project type will update it across all projects where it's used. Consider the impact on existing reports before making changes.

#### Deleting Project Types (Version 2.0+)

To remove a project type:

1. Locate the project type in the list
2. Click the "Delete" icon next to the project type
3. Confirm the deletion when prompted


### Report Standards

Report Standards define the compliance frameworks or methodologies used in your security assessments, such as OWASP Top 10 or NIST guidelines.

#### Adding Report Standards

To add a new report standard:

1. Navigate to the Configuration page
2. Go to the Report Standards section
3. Click "Add Report Standard"
4. Enter the standard name (e.g., "OWASP Top 10 2021", "NIST SP 800-53")
5. Click "Save" to add the standard

#### Editing Report Standards (Version 2.0+)

To modify an existing report standard:

1. Find the standard in the list
2. Click the "Edit" icon next to the standard
3. Update the name as needed
4. Click "Save" to apply your changes

#### Deleting Report Standards (Version 2.0+)

To remove a report standard:

1. Locate the standard in the list
2. Click the "Delete" icon next to the standard
3. Confirm the deletion when prompted

## Best Practices for Configuration Management

### Standardization

- Create a consistent naming convention for project types and standards
- Document your configuration choices for team reference
- Limit the number of project types to maintain clarity
- Use widely recognized standards when possible

### Maintenance

- Periodically review and update your configurations
- Remove unused project types and standards
- Ensure configurations align with your current service offerings
- Consider version numbers in standard names (e.g., "OWASP Top 10 2021" vs "OWASP Top 10 2017")

### Permissions

Only users with administrative privileges or the "Manage Configurations" permission can modify these settings. This restriction helps maintain consistency in your APTRS environment.