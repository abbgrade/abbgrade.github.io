---
layout: post
title: PsBuildTasks - Streamline PowerShell Module Development
tag: PsBuildTasks InvokeBuild Pester VSCode GitHubActions
draft: true
preview: /assets/psbuildtasks-solution.drawio.png
excerpt_separator: <!--more-->
---

Previously I explained [aspects of a PowerShell module](2022-04-15-reuse-in-powershell-module-development.md).
Now i'll introduce [PsBuildTasks](https://github.com/abbgrade/PsBuildTasks): A repository that provides reusable components for powershell module development and maintenance.
I'll explain how to focus on features of you PowerShell module and streamline the "overhead" like release management and testing.

<!--more-->

As a reminder the reusable aspects from the previous blog post are displayed in the following figure. In grey, what's potentially automatable and in green, what's already implemented and available.

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
Currently there are two implementations: For PowerShell-native modules and for binary modules.
But both share the same task names and can be used from other components.
For binary modules the documentation is used from the *docs* directory.
For PowerShell-native modules the documentation is generated into the *docs* directory.

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

Then VS Code tasks to access these *InvokeBuild Tasks* and *Pester Tests* easily.
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

There is a workflow to automate build validation, which includes build and testing.
Another one is for publishing into PowerShell Gallery.
Depending on the [git-flow](https://github.com/nvie/gitflow) defaults for branch names, it will be a release or a pre-release.
Workflows like build, test and publish have different dependencies like certain PowerShell modules. They can be installed with the InvokeBuild tasks *InstallBuildDependencies*, *InstallTestDependencies* and *InstallReleaseDependencies*.

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
The GitHub Actions can be included using the integrated *uses* keyword.
The VsCode tasks and InvokeBuild tasks can be updated using other InvokeBuild tasks.

Feel free to try in your (next) module and give me some feedback.
