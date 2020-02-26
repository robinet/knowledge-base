---
author: Andres Robinet
date: 2020-02-26T17:38:51.644Z
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
    - [:page_facing_up: `README.md`](#pagefacingup-readmemd)
    - [:page_facing_up: `.gitignore`](#pagefacingup-gitignore)
    - [:page_facing_up: `.gitattributes`](#pagefacingup-gitattributes)
    - [:page_facing_up: `gitversion.yml`](#pagefacingup-gitversionyml)
    - [:open_file_folder: `artifacts`](#openfilefolder-artifacts)
      - [:file_folder: `artifacts/output`](#filefolder-artifactsoutput)
        - [Projects that produce binaries / dist files](#projects-that-produce-binaries--dist-files)
        - [Projects that produce and publish a docker image](#projects-that-produce-and-publish-a-docker-image)
      - [:file_folder: `artifacts/packages`](#filefolder-artifactspackages)
      - [:file_folder: `artifacts/release`](#filefolder-artifactsrelease)
      - [:file_folder: `artifacts/tests`](#filefolder-artifactstests)
      - [:file_folder: `artifacts/tmp`](#filefolder-artifactstmp)
      - [:file_folder: `artifacts/logs`](#filefolder-artifactslogs)
    - [:open_file_folder: `build`](#openfilefolder-build)
      - [:file_folder:  `build/azure-devops`](#filefolder-buildazure-devops)
      - [:file_folder: `build/docker`](#filefolder-builddocker)
    - [:file_folder: `docs`](#filefolder-docs)
    - [:file_folder: `src`](#filefolder-src)
    - [:file_folder: `tests`](#filefolder-tests)
    - [:file_folder: `tools`](#filefolder-tools)

## To Do

- [ ] Expand artifacts sections (especially `packages` and `release`)
- [ ] Describe and expand `build`
- [ ] Local builds vs CI builds, what's the guideline regarding the use of Azure DevOps tasks
- [ ] Describe and expand `docs`, `src`, `tests`, and `tools`

## Version

`0.1.0-alpha.1`

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

### :page_facing_up: `README.md`

**Required.** Repository *README* file

Team is advised to follow suggestions from [Make a README](https://www.makeareadme.com/#suggestions-for-a-good-readme)

> **:bulb: TIP**
> [Wikipedia](https://en.wikipedia.org/wiki/README) has a good summary about this and other common files distributed with software

### :page_facing_up: `.gitignore`

**Required.** Not strictly required by git, but so helpful that is considered mandatory

> **:link: SEE ALSO**
>
> - [GitHub: Ignoring files](https://help.github.com/en/github/using-git/ignoring-files) for a general overview/reference about `.gitignore` files

### :page_facing_up: `.gitattributes`

**Optional.** Standard git customization file

> **:link: SEE ALSO**
>
> - [Customizing Git - Git Attributes](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes) in the official git docs
> - [.gitattributes Best Practices](https://rehansaeed.com/gitattributes-best-practices/) from Muhammad Rehan Saeed
> - [Be a Git ninja: the .gitattributes file](https://medium.com/@pablorsk/be-a-git-ninja-the-gitattributes-file-e58c07c9e915) for practical use cases

### :page_facing_up: `gitversion.yml`

**Optional.** Configuration file for GitVersion

[GitVersion](https://github.com/GitTools/GitVersion) is the recommended tool to manage software versioning for code in Git repositories

### :open_file_folder: `artifacts`

**Ignore explicitly**. Used to store binary / compiled / minified results of running a *build*, typically in the context of a CI/CD pipeline

Everything necessary to *deploy / release* the software will be generated / copied and stored in the `artifacts` folder during the *build* process

> **:warning: WARNING**
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

#### :file_folder: `artifacts/output`

Build output of each project

##### Projects that produce binaries / dist files

Each project will have a dedicated folder `artifacts/output/<project-id-N>` where `project-id-N` is a string or numeric value to identify one of the projects in the repo

- Binary files for .NET projects (.dll, .exe)
- Binary files for Java projects (.jar, .war)
- Bundled / minified app for Node.js projects

##### Projects that produce and publish a docker image

Text file containing the fully-qualified image name: `project-id-N.dockerimage.txt`

For example, if the `zookeeper` project would follow this standard, there would be a `zookeeper.dockerimage.txt` file with single-line contents: `quay.io/signalfuse/zookeeper:3.4.5-3`

#### :file_folder: `artifacts/packages`

Packages produced by all projects in the repo

- NuGet packages produced by the build (.nupkg)
- npm packages produced by the build (.tgz)
- Maven packages produced by the build (.pom)

#### :file_folder: `artifacts/release`

Scripts, tools and installers needed for the *release process*

Sample content:

- Cron scripts / templates used for scheduling
- Windows service installation/configuration scripts
- IIS management scripts
- Configuration template processors (to convert .ext.template into .ext)
- General purpose bash / powershell scripts

#### :file_folder: `artifacts/tests`

Test results produced by the build

#### :file_folder: `artifacts/tmp`

Temporary files generated during build

#### :file_folder: `artifacts/logs`

Build logs (binary or otherwise)

### :open_file_folder: `build`

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

#### :file_folder:  `build/azure-devops`

#### :file_folder: `build/docker`

### :file_folder: `docs`

Repository documentation

Sample content:

- Additional details and references from [README.md](#fa-file-gitattributes)
- Project-specific wiki pages

### :file_folder: `src`

This is where source code lives

### :file_folder: `tests`

This is where unit test projects and integration test projects live

### :file_folder: `tools`

Include any tools that may be necessary during development, or during a build (e.g. Portable Git, LINQPad, extensions)
