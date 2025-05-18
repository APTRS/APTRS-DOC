# Dashboard

The Dashboard provides a comprehensive overview of your projects and retests, tailored specifically to the logged-in user. It serves as your personal command center, displaying only the projects and retests that are relevant to you as the owner.

## Dashboard Overview

The Dashboard has been enhanced to provide at-a-glance statistics and detailed project information organized by status:

1. **Status Counters** - The top section displays count summaries of both projects and retests by status:
    - On Hold
    - In Progress
    - Delayed
    - Upcoming

2. **Project Status Sections** - Below the counters, projects are organized into sections by their status:
    - Each section displays projects where you are the owner
    - Projects include key information such as project name and relevant dates
    - Dates shown include start date, end date, and any status change dates

![Dashboard Page with Status Counters](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/dashboard.png)

## Projects Display Logic

Unlike the Projects View Page, which lists all projects within APTRS, the Dashboard is personalized to show only:

- Projects where you are the assigned owner
- Projects that match specific status criteria (On Hold, In Progress, Delayed, Upcoming)
- Completed projects that have active retests assigned to you

This filtered view helps you focus on what needs your attention without being distracted by projects managed by other team members or completed projects with no pending actions.

## Retest Integration

The Dashboard intelligently integrates retest information:

- Projects with active retests appear even if the original project is marked as completed
- If you're assigned as the owner of a retest but not the original project, the project will still appear on your Dashboard
- Retest status is clearly indicated alongside the project

## Benefits of the New Dashboard

The redesigned Dashboard provides several key benefits:

- **Quick Status Assessment**: Immediately see how many projects and retests are in each state
- **Prioritization**: Easily identify delayed or upcoming projects that need attention
- **Comprehensive View**: See both projects and their associated retests in one unified interface
- **Personalized Experience**: View only what's relevant to you as a user

This personalized approach ensures you can quickly assess your workload, prioritize tasks, and track important project milestones without having to filter through projects that don't require your attention.