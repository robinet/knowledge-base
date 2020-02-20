---
author: Andres Robinet
date: 2020-01-05T05:33:42.648Z
draft: true
puppeteer:
  printBackground: true
  displayHeaderFooter: true
  format: A4
---

# Git repository layout

- [Git repository layout](#git-repository-layout)
  - [To Do](#to-do)
  - [Version](#version)
  - [Purpose](#purpose)
  - [Proposed layout](#proposed-layout)
    - [:fa-file: `README.md`](#fa-file-readmemd)
    - [:fa-file: `.gitignore`](#fa-file-gitignore)
    - [:fa-file: `.gitattributes`](#fa-file-gitattributes)
    - [:fa-file: `gitversion.yml`](#fa-file-gitversionyml)
    - [:fa-folder-open: `artifacts`](#fa-folder-open-artifacts)
      - [:fa-folder: `artifacts/output`](#fa-folder-artifactsoutput)
        - [Projects that produce binaries / dist files](#projects-that-produce-binaries--dist-files)
        - [Projects that produce and publish a docker image](#projects-that-produce-and-publish-a-docker-image)
      - [:fa-folder: `artifacts/packages`](#fa-folder-artifactspackages)
      - [:fa-folder: `artifacts/release`](#fa-folder-artifactsrelease)
      - [:fa-folder: `artifacts/tests`](#fa-folder-artifactstests)
      - [:fa-folder: `artifacts/tmp`](#fa-folder-artifactstmp)
      - [:fa-folder: `artifacts/logs`](#fa-folder-artifactslogs)
    - [:fa-folder-open: `build`](#fa-folder-open-build)
      - [:fa-folder:  `build/azure-devops`](#fa-folder-buildazure-devops)
      - [:fa-folder: `build/docker`](#fa-folder-builddocker)
    - [:fa-folder: `docs`](#fa-folder-docs)
    - [:fa-folder: `src`](#fa-folder-src)
    - [:fa-folder: `tests`](#fa-folder-tests)
    - [:fa-folder: `tools`](#fa-folder-tools)

## To Do

- [ ] Expand artifacts sections (especially `packages` and `release`)
- [ ] Describe and expand `build`
- [ ] Local builds vs CI builds, what's the guideline regarding the use of Azure DevOps tasks
- [ ] Describe and expand `docs`, `src`, `tests`, and `tools`

## Version

`git-repository-layout.1.0.0`

## Purpose

Help development teams organize git repositories

## Proposed layout

```text
README.md
.gitignore
.gitattributes
gitversion.yml
artifacts
    ...
build
    ...
docs
    ...
src
    ...
tests
    ...
tools
    ...
```

### :fa-file: `README.md`

**Required.** Repository *README* file

Team is advised to follow suggestions from [Make a README](https://www.makeareadme.com/#suggestions-for-a-good-readme)

> **:fa-lightbulb-o: TIP**
> [Wikipedia](https://en.wikipedia.org/wiki/README) has a good summary about this and other common files distributed with software

### :fa-file: `.gitignore`

**Required.** Not strictly required by git, but so helpful that is considered mandatory

> **:fa-external-link: SEE ALSO**
>
> - [GitHub: Ignoring files](https://help.github.com/en/github/using-git/ignoring-files) for a general overview/reference about `.gitignore` files

### :fa-file: `.gitattributes`

**Optional.** Standard git customization file

> **:fa-external-link: SEE ALSO**
> - [Customizing Git - Git Attributes](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes) in the official git docs
> - [.gitattributes Best Practices](https://rehansaeed.com/gitattributes-best-practices/) from Muhammad Rehan Saeed
> - [Be a Git ninja: the .gitattributes file](https://medium.com/@pablorsk/be-a-git-ninja-the-gitattributes-file-e58c07c9e915) for practical use cases

### :fa-file: `gitversion.yml`

**Optional.** Configuration file for GitVersion

[GitVersion](https://github.com/GitTools/GitVersion) is the recommended tool to manage software versioning for code in Git repositories

### :fa-folder-open: `artifacts`

**Ignore explicitly**. Used to store binary / compiled / minified results of running a *build*, typically in the context of a CI/CD pipeline

Everything necessary to *deploy / release* the software will be generated / copied and stored in the `artifacts` folder during the *build* process

> **:fa-exclamation-circle: WARNING**
>
> - The `artifacts` folder is created by the build process
> - This folder should be explicitly ignored via [.gitignore](#fa-file-gitignore) to avoid mistakes, since everything created in it must never be commited / pushed

```text
artifacts
    output
        <project-id-1>
        <project-id-2>
        ...
    packages
        <project-id-1>.<package-version>.nupkg
        <project-id-1>.<package-version>.tgz
        <project-id-1>.<package-version>.pom
        ...
        <project-id-2>.<package-version>.nupkg
        <project-id-2>.<package-version>.tgz
        <project-id-2>.<package-version>.pom
        ...
    release
    tests
    tmp
    logs
```

#### :fa-folder: `artifacts/output`

Build output of each project

##### Projects that produce binaries / dist files

Each project will have a dedicated folder `artifacts/output/<project-id-N>` where `project-id-N` is a string or numeric value to identify one of the projects in the repo

- Binary files for .NET projects (.dll, .exe)
- Binary files for Java projects (.jar, .war)
- Bundled / minified app for Node.js projects

##### Projects that produce and publish a docker image

Text file containing the fully-qualified image name: `project-id-N.dockerimage.txt`

For example, if the `zookeeper` project would follow this standard, there would be a `zookeeper.dockerimage.txt` file with single-line contents: `quay.io/signalfuse/zookeeper:3.4.5-3`

#### :fa-folder: `artifacts/packages`

Packages produced by all projects in the repo

- NuGet packages produced by the build (.nupkg)
- npm packages produced by the build (.tgz)
- Maven packages produced by the build (.pom)

#### :fa-folder: `artifacts/release`

Scripts, tools and installers needed for the *release process*

Sample content:

- Cron scripts / templates used for scheduling
- Windows service installation/configuration scripts
- IIS management scripts
- Configuration template processors (to convert .ext.template into .ext)
- General purpose bash / powershell scripts

#### :fa-folder: `artifacts/tests`

Test results produced by the build

#### :fa-folder: `artifacts/tmp`

Temporary files generated during build

#### :fa-folder: `artifacts/logs`

Build logs (binary or otherwise)

### :fa-folder-open: `build`

```text
build
    build.ps1
    build.sh
    azure-devops
        <project-id-1>.azure-pipelines.yml
        <project-id-2>.azure-pipelines.yml
        ...
        <template-id-1>.yml
        <template-id-2>.yml
        ...
    docker
        <project-id-1>.dockerfile
        <project-id-2>.dockerfile
        ...
        <project-id-1>.<override-id>.docker-compose.override.yml
```

#### :fa-folder:  `build/azure-devops`

#### :fa-folder: `build/docker`

### :fa-folder: `docs`

Repository documentation

Sample content:

- Additional details and references from [README.md](#fa-file-gitattributes)
- Project-specific wiki pages

### :fa-folder: `src`

This is where source code lives

### :fa-folder: `tests`

This is where unit test projects and integration test projects live

### :fa-folder: `tools`

Include any tools that may be necessary during development, or during a build (e.g. Portable Git, LINQPad, extensions)
