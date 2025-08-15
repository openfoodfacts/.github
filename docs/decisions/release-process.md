# Release Process

## Context and Problem Statement

For most of the Open Food Facts projects we operate with a Staging and Production environment. We typically deploy to Staging every time a change is merged to the default (typically `main`) branch, whereas the Production deployment only happens when we create a Release.

A number of project use the [release-please](https://github.com/googleapis/release-please) action to manage this. This action creates a Release PR which is appended to as each other PR is merged into `main`. Merging the Release PR then triggers a release.

This process is convenient but it does have a few drawbacks:
* It is not possible to use a `GITHUB_TOKEN` on the release-please action because GitHub then prevents the subsequent deployment action from running when the Release PR is merged. It is therefore necessary to use a personal access token on this action
* The act of merging the Release PR creates a further change to the code, triggering another build, and it is this new build that is deployed to Production. Hence, what is deployed to Production isn't strictly the same as what was tested in Staging. This could cause issues if third party package dependencies have changed.

This document considers options to address this.

## Decision Drivers

* It should be possible to generate Releases and Release notes / change logs with minimal effort
* The risk of the Production deployment having a bug where the Staging deployment did not should be minimized
* The process should be easy for new contributors to understand

## Considered Options

* Continue to use Release Please as is
* Manually release using GitHub Releases
* Customize Release Please to tag the previous successfully staged build for production deployment

## Decision Outcome

Chosen option: "Continue to use Release Please as is", because it is most widely adopted and understood and automated tests should catch most issues that might occur between the build deployed to staging and the one being built for production.

### Consequences

Each project should use the release-please action to generate new releases following semantic versioning conventions.

Automated tests must be run and verified on the release candidate build before it is deployed.

## Pros and Cons of the Options

### Use Release Please

Continue to use the release please process and create a merge commit when a release is approved.

* Good: Existing process is well understood
* Good: Semantic version number is created automatically and can be written back to the repository
* Good: Flexible configuration of the change log, which is stored in the repository
* Good: Additional assets can be added to the release for production if these are not required in staging
* Bad: Build deployed to Production is not the same one that was previously tested in Staging
* Bad: An additional build is triggered as part of the release

### Use GitHub Release

With this process a Release is created via GitHub. The version (tag) has to be set manually. It is possible to automatically generate the release notes from commits to `main`. Publishing the release creates a tag on the commit that was previously deployed to staging.

* Good: Can deploy the same build to Production as was tested in Staging
* Good: Less builds
* Good: Does not rely on actions outside of GitHub's core functionality
* Bad: Version number has to be determined manually
* Bad: Version number and change log is not available in the repo itself

### Release Please tagging previous build

* Good: Can deploy the same build to Production as was tested in Staging
* Good: Semantic version number is created automatically and can be written back to the repository
* Bad: Creates a unique implementation
* Bad: Version number and change log won't be committed to the deployed build
