# Project Summary

The Project Summary provides a comprehensive overview of your project with key details including status, dates, team information, and descriptions. It's the central hub for managing project status and accessing essential project information.

![Project Summary Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/project-summary.png)

## Key Features

### Project Status Management

The Project Summary displays the current status of your project and allows authorized users to change it:

- **Mark as Complete**: Finalize a project when testing is finished

    - Automatically sends email notification to customer
    - Prevents further vulnerability additions
    - All vulnerabilities are automatically marked as published when project is completed

- **Put On Hold**: Temporarily pause a project

    - Requires reason for hold (displayed to customer)
    - Automatically sends notification email with reason to customer


### Project Information

The summary displays essential project details:

- Company name
- Project type and testing type
- Standards being followed (e.g., OWASP TOP 10 2021)
- Start and end dates
- Project status indicator

### Project Team Management

- View current project owners
- Edit project owners directly from summary (with appropriate permissions)
- Multiple project owners can be assigned for collaboration

### Project Description

- View project exception details
- Read full project description with testing scope and objectives
- Access rich text editor for description updates

## Status Behavior

- Project status affects both the main project and any retests
- Project and retest share the same status (Upcoming, In Progress, Delayed, On Hold, Completed)
- Status changes trigger appropriate notifications

## Edit Capabilities

Users with proper permissions can:

- Edit project details directly from the summary page
- Update project owners
- Modify project descriptions

!!! note "Permission Requirements"
    Editing project owners requires admin privileges or the "Assign Projects" permission