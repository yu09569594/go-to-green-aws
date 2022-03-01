## Hello world (config.0.yml)

First, branch the repository. Then `mkdir .circleci` to say hello world!

To be honest, this stage is one of the biggest challenge for beginners, because file name and syntax of the are quite important.
Starting from successful hello world helps you good start.

Syntax can be checked using `circleci config validate` command.

## Meaningful things (config.1.yml)

Do meaningful things then. Here, we will do dependency installation (`yarn install`), and build a webpacked script.

Four gimmicks here:

- Code checkout from VCS. Without this you will end up with an empty executor.
- Dynamic calculation of version number. In this way you can include the job number and digest of Git commits in version number, and easily distinguish which version is built in which job with which Git commit.
- Arming dependency cache so that jobs triggered in the future can take advantage of caches for better execution speed.
- Storing deliverable as artifacts.

## Rebuilt with SSH (config.2.yml)

Use "Rebuild with SSH" to debug.

## Workflows and parallelism (config.3.yml)

Next step is to implement tests.

To do so, we will do four things.

- Configure sequential jobs to use the same results of dependency resolution in tests and builds: For better trustworthiness of tests.
- Configure parallel jobs to run tests besides builds.
- Configure parallelism to distribute tests.
- Use larger resource class in tests to use more computation resources in tests.

## Docker and orbs (config.4.yml)

Let us package a test-passing artifact using Docker.

Docker image building is quite easy with CircleCI's official orb.
We just need to call a job pre-defined in the orb, with custom parameters (such as image name and tag name) to tune the behaviour of the job.

## Deployment (config.5.yml)

Herein we deploy the change with Helm.

First, define the job for deployment.
This is where you declare the procedure of actual deployment operation.
You might want to author your own orb to abstract the deployment operation.

Then, configure workflows.
We will add two jobs hereby: An approval job, and the actual deployment job following the approval.

The approval job is a functionality provided by CircleCI.
The feature provides a chance to intervene automated processes.
For this particular example, CircleCI provides a chance for a last-minute call to abort deployment.

After approval is given, actual deployment happens.
The deployment job takes advantage of contexts to store tokens for authentications and authorizations.
By restricting access to the context, it also is able to restrict who can conduct deployments.
