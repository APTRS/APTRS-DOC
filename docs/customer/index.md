# Customer Portal

Starting from version 2.0, APTRS provides customers with a dedicated login portal to access their project information and security reports.

![Customer Dashboard](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer-dashboard.png)

## Dashboard Features

The customer dashboard provides a comprehensive overview of security assessment status:

- **Project Status Cards**: View counts for active, upcoming, delayed, and on-hold projects
- **Security Metrics**: See overall security score and remediation progress
- **Finding Trends**: Track vulnerability trends over the past 4 months
- **Latest Findings**: Review the 10 most recent security findings
- **Active Projects & Retests**: Monitor ongoing assessment activities

## Project Views

Customers can view their projects organized by different statuses for easy tracking and management.

### Project Status Overview

![Customer Project View](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer-project-view.png)

The Projects section provides a consolidated view of all projects with filtering options:

- **Active Projects**: Currently in progress assessments
- **Upcoming Projects**: Scheduled future assessments
- **Delayed Projects**: Assessments behind schedule
- **On-Hold Projects**: Temporarily paused assessments
- **Completed Projects**: Finished assessments

### Project Details View

![Project Details View](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer-project-details.png)

When customers select a specific project, they can access a comprehensive project details page with the following features:

#### Project Information

- **Project Header**: Displays the project name and "Back to Projects" navigation
- **Project Status**: Visual indicator showing project status (Completed, Active, etc.)
- **Testing Type**: Shows methodology used (White Box, Black Box, etc.)
- **Project Timeline**: Displays project start and end dates

#### Navigation Tabs

The project details page includes several tabs for easy navigation:
- **Overview**: General project information and summary
- **Vulnerabilities**: List of all discovered security findings
- **Scope**: The assessment targets and boundaries
- **Retests**: Remediation verification status (when applicable)
- **Reports**: Access to downloadable reports

#### Project Summary Section

The Summary section provides:
- **Description**: Detailed information about the project's purpose and testing methodology
- **Project Exceptions**: Any special considerations or exclusions
- **Project Type**: The type of security assessment conducted
- **Standards**: Applied security standards (e.g., OWASP TOP 10 2021, 2017)

#### Security Assessment Overview

Displays a visual summary of security findings by severity:
- Color-coded counters for Critical, High, Medium, Low, and Informational findings
- Quick visual reference of the project's overall security posture

#### Project Team

Lists security professionals assigned to the project.

#### Timeline

Shows key project dates:
- Start Date
- End Date

#### Recent Findings Table

Provides a detailed list of vulnerabilities with:
- **Finding Name**: Title of the security vulnerability
- **Severity**: Impact level (Critical, High, Medium, Low, Informational)
- **Status**: Current state (Vulnerable, Confirm Fixed)
- **Discovery Date**: When the vulnerability was identified

### Vulnerability Details


![Projects Vulnerability](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer-projects-Vulnerability.png)
Customers can click on individual vulnerabilities to view comprehensive details:

- **Vulnerability Description**: Detailed explanation of the security issue
- **Instances**: Where the vulnerability was found in the application
- **Proof of Concept**: Demonstration of how the vulnerability can be exploited
- **Impact**: Business and security consequences
- **Remediation Steps**: Recommended fixes with code examples when applicable
- **References**: Links to industry resources explaining the vulnerability

!!! tip "Vulnerability Management"
    While customers cannot modify vulnerability information, they can use the detailed findings to prioritize their remediation efforts and track progress through the retest process.

### Past Projects View

![Past Projects View](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer-past-projects.png)

The Past Projects section allows customers to:

- View all completed security assessments in a centralized location
- See key project information at a glance:
  - Project name
  - Project dates (start and end)
  - Project type (Web Application, Internal Network, etc.)
  - Testing type (White Box, Black Box, etc.)
  - Status (Completed)
- Access detailed project information via "View details" link

#### Sorting and Filtering

Customers have robust options to organize their past projects:

- **Filters**: Click the "Filters" button to narrow down projects by:

    - Date range
    - Project type
    - Testing methodology
    - Custom filters

- **Sort**: Use the "Sort" button to order projects by:

    - Date (newest/oldest first)
    - Project name (A-Z/Z-A)
    - Project type
    - Completion status

These options make it easy to find specific past assessments when needed for reference, compliance purposes, or historical trend analysis.

### Retest Information

For projects that include retests, customers can:

- View the retest status aligned with the project status
- See planned retest dates and windows
- Track remediation progress
- Monitor findings that have been successfully fixed
- Review findings that still require remediation

!!! tip "Status Tracking"
    The project status is calculated based on retest dates when retests are active, providing real-time visibility into remediation progress.

### Filtering Options

Customers can filter projects by:

- Status (Active, Upcoming, Delayed, On-Hold, Completed)
- Date range
- Project type
- Assessment scope

## Available Actions

Customers can:

- Download project reports (PDF and Excel formats only)
- View vulnerability details
- Track project timelines and status
- Monitor remediation progress
- Update profile information
- View detailed descriptions of security findings
- Access remediation guidance for each vulnerability

## Profile Management

Customers can edit certain aspects of their profile:

- Profile photo
- Password
- Display name

!!! note "Email Changes"
    Email addresses cannot be modified by customers. Contact your security provider for email address changes.

## Report Access

- Access and download all finalized reports
- Reports are available in PDF and Excel formats
- Word (DOCX) reports are not available to customers