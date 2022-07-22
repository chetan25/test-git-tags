# Git and Jenkins Workflow for Release

## Jenkins Workflow

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

- NOTE - We could have just used `Freestyle project` for both and use `Post-build Actions` to trigger a different Job. But since we need a Jenkins file, we went with a Pipeline.

#### Current Repo Jenkins flow - Job 1

- This repo is running the first Job, and has a Jenkins file that release this package using `Semantic Release` and stores the output to be passed as parameter to the second job.
- We use the semantic release `exec` package to store the version released in a temp file and read that in Jenkins. See the full semantic release configuration in the `.releaserc` file.
  ```js
  [
    "@semantic-release/exec",
    {
      "verifyReleaseCmd": "echo ${nextRelease.version} > .VERSION"
    }
  ],
  ```
- After that we just remotely execute the second job using a `curl` command and the url for the second job. We pass the parameterized argument for the second job as data.
  ```js
  sh """curl -X POST -u $USERNAME:$PASSWORD http://localhost:9090/job/TestDraftPR/buildWithParameters?token=myToken --data 'VERSION=${REPO_LATEST_TAG}' --data 'PR_NAME=${PR_NAME}'"""
  ```
- Token here is hardcoded(since it only in my local) and set in the second job. But we can put that in secrets for Production
- Refer the `Jenkinsfile` for the detail steps.

#### Other Repo Jenkins flow - Job 2

> The other repo is [here](https://github.com/chetan25/git-jenkins-multi-repo)

- This Jenkins job is a parameterized one that has two variables, `VERSION` and `PR_NAME`.
- We have also enabled the `Trigger builds remotely (e.g., from scripts)` from the `Build Trigger` steps in the configuration and set the token to be passed by the first job. This step also gave us the URL to be used to trigger this job remotely.
- Example `http://localhost:9090/job/TestDraftPR/buildWithParameters?token=myToken`
- This just reads the parameters and then checkouts out a new branch as the `PR_NAME`. We use `git` for the basic operations.
- Does a dummy react update.
- Commits the file and does a remote push.
- Then we use the github cli `gh` and not `git` command, to create a draft PR.
  ```js
  gh pr create --title "version-update" --body "Pull request body"
  ```
- `gh` and `git` are already present in Jenkins, since we installed the git plugin.
- Here is the full [Jenkins](https://github.com/chetan25/git-jenkins-multi-repo/blob/main/Jenkinsfile) file

## Github action Workflow

- This is quite simple we just use the sematic release command set in our package.json file.

```js
 "s:release": "dotenv cross-var cross-env GITHUB_TOKEN=%GITHUB_TOKEN% DEBUG=semantic-release* semantic-release --no-ci",
```

- Note we are using 3 packages here `dotenv, cross-var, cross-env` just to use the environment variable set in a file and pass it as a new run time environment variable for this semantic release command. Could not find a better way to do it.

- A simple action would look like
  ```yml
  env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    steps:
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 16
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - run: npm ci
      name: Release a new version
      run: npm run s:release
  ```
