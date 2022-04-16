---
layout: post
title: Reuse in PowerShell Module Development
tag: PsBuildTasks Pester InvokeBuild VSCode platyPs GitHubActions SemVer KeepAChangeLog GitFlow
excerpt_separator: <!--more-->
preview: /assets/psbuildtasks-1.drawio.png
---

In the last years I learned to reuse some code between PowerShell modules, not like Cmdlets imported from other modules but structure and tools.
I'll explain how to improve your modules and your development process.

<!--more-->

Basically a PowerShell module is only a manifest file like `myModule.psd1` and come code implementing it `myModule.psm1`. None of that allow much reuse, since both is custom for every module.

![PsBuildTasks Overview - Stage 1](/assets/psbuildtasks-1.drawio.png)

If you maintain that module over a longer time range. It's a good idea to use a single file per Cmdlet, to add a readme file and to add [Pester](https://github.com/pester/pester) tests. Still most of that is individual for that module. But you could automatically load the `psd1` files per Cmdlet and publish them based on the directory structure.

```
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
└─test
  | Cmdlet2.Tests.ps1
```

![PsBuildTasks Overview - Stage 2](/assets/psbuildtasks-2.drawio.png)

In case your module is implemented in C# instead of PowerShell, before using and testing it, you need to build `myModule.dll` file.
Therefore you should use [InvokeBuild](https://github.com/nightroman/Invoke-Build) and VS Code tasks to simplify and automate it.
This more or less the same for all C#-based modules: removing some files to cleanup, running `dotnet build`. The test task depends on the build task ...

![PsBuildTasks Overview - Stage 3](/assets/psbuildtasks-3.drawio.png)

The next stage is to run it as a open source project, that is used by people who do know much about your module.
You should add documentation and instructions, a license information and publish it to PowerShell Gallery.
Documentation should be based on [platyPs](https://github.com/PowerShell/platyPS). The content is specific for your module, but the generation and export is not.
The license may be the same for all your modules, but won't change often anyway.
The publishing to PowerShell Gallery can be reused but needs configuration per module.
Installation and contribution instructions should not deviate between your modules.

![PsBuildTasks Overview - Stage 4](/assets/psbuildtasks-4.drawio.png)

When the project becomes more mature and more people or projects depend on it, think about release management, branching models, CI/CD.
Therefore I recommend [Semantic Versioning](https://semver.org) and [keep a changelog](https://keepachangelog.com) and [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/). These are more or less conventions, but based on that, you can develop your automation workflows, that do build validation, release and pre-releases.

![PsBuildTasks Overview - Final Stage](/assets/psbuildtasks.drawio.png)

You see that your module became quite complex, but most of the complexity can be automated. You should understand that parts but then, they won't slow you down compared to the benefit.
