# Release Lifecycle

This documentation contains information how release cycles work in our team and instructions on how to publish a new release.

## Environments
We have two environments: `dev` and `prod`. A `stage` environment is currently in progress.

### Dev
The `dev` environment includes new changes to be implemented in the release, such as new endpoints or changes to existing ones.

### Stage
After development in `dev` is completed, we test in the `stage` environment before migrating to `prod`. The `stage` branch is derived from `prod`, and new `dev` changes are added for testing.

### Prod
After successful testing in `stage`, we push changes to `prod`. The `prod` environment contains real user data, and we test with our test company. Typically, this transition should work smoothly as the database is similar to `stage`. For mobile, we conduct some tests before releasing to the stores.

## Frontend Branching
For both web and mobile frontend, we use GitFlow for branching. The following branches are used:

* **main**: Contains the latest version available in the stores and on the production website.
* **dev**: Contains changes currently in development to be included in upcoming releases.
* **feature**: Branches off `dev` for specific tasks. After completion, these are merged back into `dev`.
* **release**: Branches off `dev` for testing and bug fixes. Once tested, these are merged into both `dev` and `main`. Hotfixes are also handled here, branching off `main` and merging back into `main` and `dev` after fixing.

### Branching Workflow
1. Start new tasks by branching from `dev` to `feature`.
2. Develop in the `feature` branch and merge back into `dev` upon completion.
3. Before release, create a `release` branch from `dev` and address any bugs.
4. Merge the `release` branch into `main` when sending to stores and into `dev` for continued development.

## Development Cycle
We follow agile development practices and use a Kanban board for task tracking. Each sprint produces one release every 1-2 weeks.

### Cycle
1. Post-release, discuss features for the next release and set an estimated deadline. Create related tasks in the Kanban board's backlog column.
2. The designer creates the necessary designs and submits them for review. Once frontend developers approve, tasks move to the backend.
3. The backend team makes necessary changes, creates or edits endpoints, and submits them for review by frontend developers.
4. Frontend developers implement changes in `dev` branches and move tasks to the testing column. Upon reaching the deadline, release for testing.
5. After testing all features, migrate to `stage` and then to `prod`. Typically, migration occurs at 23:00 when fewer clients are using the apps. Incomplete tasks are moved to the next sprint.
6. If bugs are reported by clients post-release, create a branch from `main` and release the hotfix. If the hotfix takes significant time, the sprint deadline may be adjusted.

---

By following this documentation, we can ensure a smooth and structured release process for our mobile, web, and backend projects.
