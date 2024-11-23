

APTRS allows users to create and manage projects, which are typically created when conducting a pentest for a company. When setting up a project, you can associate it with a specific company, select multiple internal users as project owners, and define key details such as project type, start and end dates, and a brief description.

Once the project is created, you can later add vulnerabilities, generate reports, and track progress within the project. This feature helps organize pentesting efforts efficiently, ensuring clear project ownership and structure.

![project Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/project.png)

### Project Status
The project status is automatically calculated based on the project’s start and end dates:

- **Upcoming**: If the start date is in the future.
- **In Progress**: If the start date has passed or is today, but the end date is still in the future.
- **Delayed**: If the end date has passed.
- **Completed**: If the project is marked as completed.

If your project is completed, you can manually mark it as **Completed** from the project summary. To do this, navigate to the project from the project table or dashboard, and you’ll find an option to mark it as completed on the project summary page.


### Project Details
Once a project is created, it is not possible to change the associated company. However, you can still edit other details, such as project type, dates, description, and owners.



### Project Owners
In Version 1.0, APTRS supports assigning multiple project owners, allowing you to add as many project owners as needed. Assigning or selecting a project owner requires admin privileges or the Assign Projects permission. Otherwise, the user creating the project is automatically added as the project owner.

