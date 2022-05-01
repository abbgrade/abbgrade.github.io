---
layout: post
title: PowerShell Module Development - Reuse and Maturity
tag: PsBuildTasks Pester InvokeBuild VSCode platyPs GitHubActions SemVer KeepAChangeLog GitFlow
excerpt_separator: <!--more-->
preview: /assets/psbuildtasks-1.drawio.png
---

In the last years, I learned to reuse some code between PowerShell modules, not like Cmdlets imported from other modules but structure and tools.
I'll explain how to improve your modules and your development process.

<!--more-->

## Tier 1 - Minimum PowerShell Module

A PowerShell module is only a manifest file like `myModule.psd1` and a code file implementing it `myModule.psm1`. None of them allow much reuse since both are custom for every module.

![PsBuildTasks Overview - Tier 1](/assets/psbuildtasks-1.drawio.png)

## Tier 2 - Structure, Instructions and Testing

If you maintain that module over a longer time range, you should invest more effort.
Use a single file per Cmdlet to support analysis or change.
Add a readme file with instructions and the general concept.
Add [Pester](https://github.com/pester/pester) tests to minimize bugs. As a side effect, tests can be used as examples too.
Most of that is still individual to that module. But you could automatically load the `psd1` files per Cmdlet and publish them based on the directory structure.

![PsBuildTasks Overview - Stage 2](/assets/psbuildtasks-2.drawio.png)

## Tier 3 - Build and Automation

If you implement the module in C# (binary module), before using and testing it, you need to build `myModule.dll` file.
Therefore you should use [InvokeBuild](https://github.com/nightroman/Invoke-Build) and VS Code tasks to simplify and automate it.

![PsBuildTasks Overview - Stage 3](/assets/psbuildtasks-3.drawio.png)

## Tier 4 - Open Source and Sharing

If your project is open-source, users won't know much about it.
You should add documentation, instruction, and license information.
Additionally, publish it to PowerShell Gallery.
Documentation should be based on [platyPs](https://github.com/PowerShell/platyPS).
The content is specific for your module, but the generation and export are not.
The license may be the same for all your modules but won't often change anyway.
The publishing to PowerShell Gallery can be reused but needs configuration per module.
Installation and contribution instructions should not deviate between your modules.

![PsBuildTasks Overview - Stage 4](/assets/psbuildtasks-4.drawio.png)

## Tier 5 - Release Management and Continuous Integration

When your project becomes more mature, more people or projects depend on it - think about release management, branching models, and CI/CD.
Therefore I recommend [Semantic Versioning](https://semver.org), [keep a changelog](https://keepachangelog.com) and [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/).
These are conventions, but you can develop your automation workflows for build validation, release, and pre-release.

![PsBuildTasks Overview - Final Stage](/assets/psbuildtasks.drawio.png)

## Summary

Your module became quite complex, but you can simplify it by automation.
You should understand all parts, but they won't slow you down compared to the benefit.
More, in my next post about [streamlining the development](2022-04-16-Streamlining-PowerShell-Modules-with-PsBuildTasks.md).
