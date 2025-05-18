# Project Retest

Retests verify if vulnerabilities have been properly fixed by clients after the initial assessment.

![Retest Interface](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/retest.png)

## Key Features

- Assign specific owners to handle retest assessments
- Track fixed and unfixed vulnerabilities
- Generate separate retest reports
- Put retests on hold with documented reasons

## Status Management

- Project and retest share the same status system
- If a project has an active retest, its status is based on the retest dates
- Without a retest, status is calculated from project start/end dates
- Both project and retest can be put on hold with a documented reason

## Retest Requirements

- Can only be added to completed projects
- Only one active retest allowed at a time
- Cannot be edited after creation (but can be deleted)

## Owner Assignment

- Creator becomes owner by default
- Users with admin rights or "Assign Projects" permission can select different owners


