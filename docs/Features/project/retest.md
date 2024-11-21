Each project can have a retest task created and assigned to a user. Similar to a project, each retest includes a start and end date and has a designated retest owner. Retests allow you to specify the revalidation of the project’s vulnerabilities, ensuring that fixes have been applied as expected. Within a project, users can initiate a retest to start this process.

![Retest](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/retest.png)

Similar to project ownership, the retest owner is automatically set to the user creating the retest if they do not have admin privileges or the Assign Projects permission. Users with admin access or Assign Projects permission can select the retest owner. Once a retest task is created, it cannot be edited, though you can still delete the retest task if needed.

### Validation

- **Project Completion Requirement:** A retest task cannot be created if the project is not marked as completed. Retests are only applicable to projects that have been fully executed, as retests aim to validate resolved vulnerabilities. Therefore, a project must be completed before any retest can be initiated.
- **Single Active Retest Restriction:** Even if the project is completed, you cannot create a new retest task if there is an existing retest task that is not marked as completed. Only one active retest can be associated with a project at a time.


