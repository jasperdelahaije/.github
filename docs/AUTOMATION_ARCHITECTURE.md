# Company automation architecture

## Objective

Build a small internal operating platform that removes coordination work from the sales-to-delivery lifecycle without over-automating customer-facing decisions.

## Core components

### business-ops

Source of truth for leads, clients, projects and scheduled follow-ups. Django provides the first internal UI and Celery handles asynchronous and scheduled jobs.

### project-bootstrap

Technical project factory. Given an approved client/project codename, it creates a private repository, starter files, labels and kickoff tasks. Later it can be triggered automatically by `business-ops` when a lead becomes won.

### infrastructure

Shared observability baseline for internal services and deployed automations. Client application state and credentials stay isolated.

## Automation boundary

Automate preparation by default. Keep externally consequential actions reviewable until the workflow has proven reliable.

Examples:

- automatic: create internal follow-up tasks;
- automatic: create a project repository after an explicit Won state;
- review first: send cold outreach;
- review first: publish social content;
- review first: send proposals and invoices until templates and integrations are mature.

## First end-to-end workflow

1. Lead enters `business-ops`.
2. Follow-up date is scheduled.
3. Discovery and proposal state are tracked.
4. Jasper marks the lead `Won`.
5. `business-ops` creates a Client and Project.
6. `business-ops` calls `project-bootstrap`.
7. A private GitHub repository is created with CI, Docker baseline and kickoff issues.
8. The repository URL is saved back to the Project.
9. Monitoring is added when a service is deployed.

One explicit state transition should remove most of the repetitive setup around a new assignment.
