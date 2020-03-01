---
author: Andres Robinet
date: 2020-03-01T03:57:10.840Z
title: Git Repository Layout 0.1.0-alpha.2
puppeteer:
  displayHeaderFooter: true
  printBackground: true
  format: A4
---

# Git repository layout <!-- omit in toc -->

## Table of contents <!-- omit in toc -->

- [Version](#version)
- [Purpose](#purpose)
- [Assumptions](#assumptions)
- [Proposed layout](#proposed-layout)
  - [README file](#readme-file)
  - [.gitignore](#gitignore)
  - [.gitattributes](#gitattributes)
  - [.editorconfig](#editorconfig)
  - [.dockerignore](#dockerignore)
  - [gitversion.yml](#gitversionyml)
  - [Artifacts](#artifacts)
  - [Build](#build)
  - [Docs](#docs)
  - [Source](#source)
  - [Tests](#tests)
  - [Tools](#tools)

## TODO <!-- omit in toc -->

- [ ] Describe and expand `build/`
- [ ] Local builds vs CI builds, provide guideline regarding the use of Azure DevOps YAML pipelines and tasks
- [ ] Describe and expand `docs/`, `src/`, `tests/`, and `tools/`

## Version

`0.1.0-alpha.2`

## Purpose

Help development teams organize git repositories

## Assumptions

A git repository will contain one or more *projects*

As a result of a build, each *project* will produce:

1. At most one folder of compiled/built files
2. At most one docker image
3. One or more distributable packages

## Proposed layout

```text
README.md
.gitignore
.gitattributes
.editorconfig
.dockerignore
gitversion.yml
artifacts/
    ...
build/
    ...
docs/
    ...
src/
    ...
tests/
    ...
tools/
    ...
```

### README file

**Required.** Repository `README.md` file

Team is advised to follow suggestions from [Make a README](https://www.makeareadme.com/#suggestions-for-a-good-readme)

> **:bulb: TIP**
>
> [Wikipedia](https://en.wikipedia.org/wiki/README) has a good summary about this and other common files distributed with software

### .gitignore

**Required.** Git ignore configuration: `.gitignore`

Not strictly required by git, but so helpful that is considered mandatory

> **:link: SEE ALSO**
>
> [GitHub: Ignoring files](https://help.github.com/en/github/using-git/ignoring-files) for a general overview/reference about `.gitignore` files

### .gitattributes

**Optional.** Git file attributes configuration: `.gitattributes`

> **:link: SEE ALSO**
>
> [Customizing Git - Git Attributes](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes) in the official git docs
> [.gitattributes Best Practices](https://rehansaeed.com/gitattributes-best-practices/) from Muhammad Rehan Saeed
> [Be a Git ninja: the .gitattributes file](https://medium.com/@pablorsk/be-a-git-ninja-the-gitattributes-file-e58c07c9e915) for practical use cases

### .editorconfig

**Required.** Coding style configuration across various editors and IDEs: `.editorconfig`

> **:link: SEE ALSO**
>
> [EditorConfig](https://editorconfig.org/#file-format-details) a file format for defining coding styles

### .dockerignore

**Required.** Docker ignore configuration: `.dockerignore`

> **:link: SEE ALSO**
> [Do not ignore .dockerignore](https://medium.com/hackernoon/do-not-ignore-dockerignore-47f5fb67b448) explains the reasons behind `.dockerignore` and how to use it
> [.dockerignore file](https://docs.docker.com/engine/reference/builder/#dockerignore-file) is the official reference

### gitversion.yml

**Optional.** Configuration file for GitVersion: `gitversion.yml`

[GitVersion](https://github.com/GitTools/GitVersion) is the recommended tool to manage software versioning for code in Git repositories

### Artifacts

**Ignore in .git.** Stores the resulting files after running a *build*, typically in the context of a CI/CD pipeline

Everything necessary to *deploy / release* the software will be generated / copied and stored in the `artifacts` folder during the *build process*

> **:warning: WARNING**
>
> The `artifacts` folder is created by the build process
> This folder should be explicitly ignored via [.gitignore](#gitignore) to avoid mistakes, since everything created in it must never be committed / pushed

```text
artifacts/
    build/
        <project-id-1>/
            ...
        <project-id-2>/
            ...
        ...
    docker/
        <project-id-1>.txt
        <project-id-2>.txt
        ...
    packages/
        <package-id-1>.nupkg
        <package-id-1>.tgz
        <package-id-1>.pom
        ...
        <package-id-2>.<package-version>.nupkg
        <package-id-2>.<package-version>.tgz
        <package-id-2>.<package-version>.pom
        ...
    release/
        ...
    tests/
        ...
```

#### Artifacts - Build output

Per-project build output: `artifacts/build/<project-id-*>`

Dedicated folder  where `project-id-*` is a string value to identify one of the projects in the repo

- Binary files for .NET projects (.dll, .exe)
  - Typically, the contents of the `bin` folder
  - Published contents for web projects
- Binary files for Java projects (.jar, .war)
- Bundled / minified app for Node.js projects
  - Contents of the `dist` folder

#### Artifacts - Docker images

Per-project docker image: `artifacts/docker/project-id-*.dockerimage.txt`

Text file containing the fully-qualified image name: `hostname[:port]/username/reponame[:tag]`

> **:link: SEE ALSO**
>
> [Referencing Docker Images](https://windsock.io/referencing-docker-images) to learn about fully qualified image names (FQIN)

#### Artifacts - Package files

Packages produced by the build, across all projects in the repo `artifacts/packages/<package-id-*>.<package-extension>`

> **:bulb: TIP**
>
> Package ID's should align with project IDs, for example: `artifacts/packages/<project-id-*>.<package-version>.<package-extension>`

Examples of package extensions are:

- NuGet: `.nupkg`
- npm: `.tgz`
- Maven: `.pom`

#### Artifacts - Release support files

Scripts, tools, docs and installers needed for the *release process*: `artifacts/release`

For example:

- **Installer or general purpose scripts**, such as bash or powershell scripts
- **Release support docs** such as guides, checklists and notes about the release process
- **Executable tools**, such as template processors, portable LINQPad, etc. that are needed when running a release pipeline
- **Cron** scripts or templates used for scheduling
- **Windows Services** installation/configuration scripts
- **IIS** management scripts

#### Artifacts - Test results

Test results produced by the build: `artifacts/tests`

### Build

Build scripts and files: `build/`

```text
build/
    build.ps1
    build.sh
    azure-pipelines.yml
    docker/
        <project-id-1>.dockerfile
        <project-id-2>.dockerfile
        ...
```

#### Build - Bootstrapping scripts

Bootstrapping scripts:

- `build/build.ps1` for PowerShell (Windows)
- `build/build.sh` for BASH (Linux / Mac)

#### Build - Azure pipelines configuration

Azure DevOps YAML pipeline configuration: `build/azure-pipelines.yml`

#### Build - Docker

Docker files: `build/docker`

### Docs

Repository documentation: `docs/`

- Additional details and references
- Project-specific wiki pages

### Source

Source code: `src/`

### Tests

Unit and integration test projects: `tests/`

### Tools

Support tools: `tools/`

Include any tools that may be necessary during development, or during a build (e.g. Portable Git, LINQPad, extensions)
