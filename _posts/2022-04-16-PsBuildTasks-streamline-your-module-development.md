---
layout: post
title: PsBuildTasks - streamline your module development
tag: PsBuildTasks InvokeBuild Pester VSCode GitHubActions
draft: true
excerpt_separator: <!--more-->
---

Previously I explained [aspects of a PowerShell module](2022-04-15-reuse-in-powershell-module-development.md), now i'll focus on reuse and automate as much as possible and how to focus on features of you PowerShell module and streamline the "overhead" like release management and testing.

<!--more-->

![PsBuildTasks Structure](/assets/psbuildtasks-solution.drawio.png)

First it's the module structure, like using one file per cmdlet, loading and publishing them in the module.
Then scripts like InvokeBuild tasks, to build, test and document the module.
Then VS Code tasks to access these scripts easily.
Finally GitHub Actions to automate build validation and publishing to PowerShellGallery.

With a growing number of modules and reusable parts, it becomes messy to synchronize them. Therefore I introduced [PsBuildTasks](https://github.com/abbgrade/PsBuildTasks) as a central repository for that reusable parts and the methods to update them.

Currently it supports two types of PowerShell modules: DotNet/C#-based modules and PowerShell-native modules.

Feel free to try in your (next) module and give me some feedback.
