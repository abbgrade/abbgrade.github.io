---
layout: post
title: Streamlining PowerShell Modules with PsBuildTasks
tag: PsBuildTasks InvokeBuild Pester VSCode GitHubActions GitFlow
draft: true
preview: /assets/psbuildtasks-solution.drawio.png
excerpt_separator: <!--more-->
---

Previously I explained [aspects of a PowerShell module](2022-04-15-PowerShell-Module-Development-Reuse-and-Maturity.md).
Now I'll introduce [PsBuildTasks](https://github.com/abbgrade/PsBuildTasks): A repository that provides reusable components for PowerShell module development and maintenance.
I'll explain how to focus on features of your PowerShell module and streamline the "overhead" like release management and testing.

<!--more-->

As a reminder, see the reusable aspects of a PowerShell module in the following figure.
The grey boxes show potentially automatable aspects.
The green boxes show available features of PsBuildTasks.

![PsBuildTasks Structure](/assets/psbuildtasks-solution.drawio.png)

## Project Directory Structure

The module structure is the foundation for further automation.
It follows the [Convention over configuration paradigm](https://en.wikipedia.org/wiki/Convention_over_configuration).
It contains using one file per cmdlet, loading and publishing them in the module.

```plaintext
myModule
|
└─source
| | myModule.psd1
| | myModule.psm1
| |
| └─internal
| | | Cmdlet1.psd1
| |
| └─public
|   | Cmdlet2.psd1
|
| Changelog.md
| License
| Readme.md
```

## InvokeBuild Tasks

Foundation for all automation are [InvokeBuild](https://github.com/nightroman/Invoke-Build) tasks like *Build*, *Clean*, *Install* and *Update Docs*.
So far, there are two implementations: For PowerShell-native modules and binary modules.
But both share the same task names and can be used from other components.
The *docs* directory contains documentation in markdown format.
It's read for binary modules and exported for PowerShell-native modules to that location.

```plaintext
myModule
|
└─docs
| | Cmdlet2.md
|
└─tasks
| | Build.Tasks.ps1
|
| .build.ps1
```

## VS Code Tasks

VS Code tasks access these *InvokeBuild Tasks* and *Pester Tests*.
Therefore, it expects the tests to be in the *test* directory.

```plaintext
myModule
|
└─.vscode
| | tasks.json
|
└─tasks
| | Build.Tasks.ps1
|
└─test
  | Cmdlet2.Tests.ps1
```

## GitHub Actions

A workflow to automate build validation includes building and testing.
Publishing into PowerShell Gallery is implemented in two workflows.
Depending on the [git-flow](https://github.com/nvie/gitflow) defaults for branch names, it will be a release or a pre-release.
Workflows like build, test, and publish, have dependencies.
Dependencies like PowerShell modules. These are installed with the InvokeBuild tasks *InstallBuildDependencies*, *InstallTestDependencies*, and *InstallReleaseDependencies*.

```plaintext
myModule
|
└─tasks
| | Build.Tasks.ps1
| | Dependency.Tasks.ps1
|
└─.github
  └─workflows
    | build-validation.yml
    | pre-release.yml.yml
    | release.yml.yml
```

## Update PsBuildTasks

With a growing number of modules and reusable parts, it becomes messy to synchronize them.
Therefore I introduced [PsBuildTasks](https://github.com/abbgrade/PsBuildTasks) as a central repository for that reusable parts and the methods to update them.
Include the GitHub Actions using the integrated *uses* keyword.
Install or update VS Code and InvokeBuild tasks using other InvokeBuild tasks.

Feel free to try in your (next) module and give me some feedback.
