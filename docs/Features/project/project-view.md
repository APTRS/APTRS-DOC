# Project Management

Projects in APTRS represent individual penetration testing engagements. Each project is linked to a company and contains vulnerabilities, reports, and other assessment data.

![Project Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/project.png)

## Project View

- Shows all projects
- Filter projects by status (In Progress, Delayed, On Hold, Upcoming, Completed)
- Provides overview of all ongoing and completed assessments

## Project Status

Status is automatically determined by dates:

- **Upcoming**: Start date is in the future
- **In Progress**: Between start and end dates
- **Delayed**: End date has passed
- **Completed**: Manually marked as completed

## Project Details

- Company association cannot be changed after creation
- Other details (type, dates, description) can be edited anytime

## Project Owners

- Multiple owners can be assigned to a project
- Requires admin rights or "Assign Projects" permission
- Project creator becomes owner by default

