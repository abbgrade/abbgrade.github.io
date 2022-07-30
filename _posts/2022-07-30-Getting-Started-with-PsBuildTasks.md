---
layout: post
title: Getting Started with PsBuildTasks
tags: PsBuildTasks InvokeBuild
excerpt_separator: <!--more-->
---

Previously I explained [the vision of PsBuildTasks and how it works](2022-04-16-Streamlining-PowerShell-Modules-with-PsBuildTasks.md).
Unfortunately you need a lot about it to get it working.
This has changed since it has now it's own PowerShell module for installation.

<!--more-->

In case your module `MyModuleName` is written in PowerShell and should be cross-platform.
Only a few steps are needed.

At first you need to install the module. That can be easily done from PsGallery using the command `Install-Module -Name PsBuildTasks`.

Then PsBuildTasks container tasks need to be installed with the following commands.

```powershell
Import-Module -Name PsBuildTasks
Install-PsBuildTask -Path . -Task 'PowerShell-Matrix'
```

Next the actual features can be installed or updated with:

```powershell
$ModuleName = 'MyModuleName'
Invoke-Build -File .\tasks\PsBuild.Tasks.ps1 -Task UpdatePsBuildTasks
```

This includes [InvokeBuild](https://github.com/nightroman/Invoke-Build) tasks.
To use them, add the following to your `.build.ps1` file:

```powershell
$ModuleName = 'MyModuleName'
. ./tasks/Build.Tasks.ps1
```

Now your project should have GitHub Actions, GitHub Pages and VsCode Tasks in place.
You can build and test your module with VsCode.
GitHub Actions support build validation, releases and pre-releases to PsGallery.
GitHub Pages contain your module documentation.
