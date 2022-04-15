---
layout: post
title: Reuse in PowerShell Module Development
---

In the last years I developed quite a number of PowerShell modules and learned to reuse some code beside the actual cmdlets.
First it's the module structure, like using one file per cmdlet, loading and publishing them in the module.
Then scripts like InvokeBuild tasks, to build, test and document the module.
Then VS Code tasks to access these scripts easily.
Finally GitHub Actions to automate build validation and publishing to PowerShellGallery.

With a growing number of modules and reusable parts, it becomes messy to synchronize them. Therefore I introduced https://github.com/abbgrade/PsBuildTasks as a central repository for that reusable parts and the methods to update them.

Currently it supports two types of PowerShell modules: DotNet/C#-based modules and PowerShell-native modules.

Feel free to try in your (next) module and give me some feedback.