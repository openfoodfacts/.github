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

## Considered Options

* Continue to use Release Please
* Manually release using GitHub Releases

## Decision Outcome

TBA

Chosen option: "{title of option 1}", because
{justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

### Consequences

TBA

{Provide detail on the implications of making this decision and how any forseen problems can be mitigated}

<!-- This is an optional element. Feel free to remove. -->
### Confirmation

TBA

{Describe how the implementation of/compliance with the ADR is confirmed. E.g., by a review or an ArchUnit test.
 Although we classify this element as optional, it is included in most ADRs.}

<!-- This is an optional element. Feel free to remove. -->
## Pros and Cons of the Options

### Use Release Please

Continue to use the release please process and create a merge commit when a release is approved.

* Good: Existing process is well understood
* Good: Semantic version number is created automatically and can be written back to the repository
* Good: Flexible configuration of the change log, which is stored in the repository
* Bad: Build deployed to Production is not the same one that was previously tested in Staging
* Bad: An additional build is triggered as part of the release

### Use GitHub Release

With this process a Release is created via GitHub. The version (tag) has to be set manually. It is possible to automatically generate the release notes from commits to `main`. Publishing the release creates a tag on the commit that was previously deployed to staging.

* Good: Can deploy the same build to Production as was tested in Staging
* Good: Less builds
* Good: Does not rely on actions outside of GitHub's core functionality
* Bad: Version number has to be determined manually
* Bad: Version number and change log is not available in the repo itself
