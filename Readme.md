# Git and Jenkins Workflow for Release

#### Jenkins Workflow

- We can use Jenkins pipeline as a CI/CD pipeline, to install, lint, test and build our projects.
- We can also go a step further and interlink two Jenkins pipeline to run them one after another based on some conditions.
- Make sure you have the `Git Plugin` installed in Jenkins.
- You can verify that by navigating to the `Manage Jenkins ==> Manage Plugins` section on the dashboard.
- Now there are many ways to create a job in Jenkins that can be hooked to Github repo. We will be using Pipeline:

  - Pipeline - Orchestrates long-running activities that can span multiple build agents. Suitable for building pipelines (formerly known as workflows) and/or organizing complex activities that do not easily fit in free-style job type.

- We will create the first Job as Pipeline project that will do the following:

  - Release the new version of the lib on github.
  - Assume we also release that version to npm.
  - Than it triggers a new Job, and passes the newly created version to it as parameter on successful build.
  - This trigger will happen via curl request, as the second job is invoked as a script.

- The second Job, will be a simple parameterized Pipeline that

  - Accepts an input as a version number and a package name and installs it.
  - Creates a PR with the new changes.

- NOTE - We could have just used `Freestyle project` for both and use `Post-build Actions` to trigger a different Job.

- This repo is running the first Job, and has a Jenkins file that release this package using `Semantic Release` and stores the output to be passed as parameter to the second job.
-
