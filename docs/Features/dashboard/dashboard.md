# Dashboard


The Dashboard provides a personalized view of projects based on the logged-in user’s ownership and project status. Unlike the Projects View Page, which lists all projects within APTRS, the Dashboard only displays projects where the logged-in user is the owner and the project is not marked as completed. The Dashboard shows projects with the following statuses: Delay, In Progress, or Upcoming, as long as the user is the owner of those projects.

In addition to active or upcoming projects, the Dashboard also displays completed projects if the project has an active retest. Even if a project is marked as completed, it will be shown in the Dashboard if it has any upcoming, in-progress, or delayed retests where the logged-in user is the owner of the retest task.

This also applies in cases where the user is the owner of the retest task for a completed project, even if the user is not the owner of the original project. In such cases, the project will still appear on the Dashboard, allowing the user to view and manage their active retest tasks.

![Dashboard Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/dashboard.png)