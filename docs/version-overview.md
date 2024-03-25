## Versioning Pages.

[Home](version-index.md) | [Quick Start](version-quickstart.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)


## Versioning Overview

To get up and running really quickly but without much information there is the [Quick Start](quickstart.md) guide.  The overview provides more detailed information and the reference provides reference information for when you are using the versioning tool.

### Command Line Tool

Versioning can be referenced from the assembly in your own code or through the command line tool.  The command line tool is designed to allow you to automate versioning tasks and be included in pipelines and DevOps automations.  

The command line tool is pliskytool.exe and is used for most operations.  See the [Command Line Reference](version-commandline.md) for full syntax and information.

### Snippets

These are common snippets used to version on a CI and Release Pipeline.  Items in [] with capitals should be replaced by your own values.

#### CI Versioning Snippet
Assumes that Versonify is in [YOURPATH] and that you are using a file for the matches called AutoVersion.txt

```yaml
- task: PowerShell@2
  displayName: 'CI Versioning'
  condition: not(eq(variables['Build.Reason'],'PullRequest'))
  inputs:
      targetType: 'inline'
      script: |
            # Versioning Powershell.
            [YOURPATH]Versonify.exe UpdateFiles -Root=$(build.sourcesDirectory)\src\ -NO -VS=[YOURPATH]\[YOURFILENAME].vstore -Increment -MM=$(build.sourcesDirectory)\[YOURPATH]\AutoVersion.txt
            [YOURPATH]Versonify.exe Passive -VS=[YOURPATH]\[YOURFILENAME].vstore -O=file
            $storedVersion = Get-Content pver-latest.txt
            
            Write-Host "Build Version Is: $storedVersion"
            Write-Host "##vso[task.setvariable variable=buildVersionNumber;]$storedVersion"
```

The PR build can be used to queue the next version number that will be used on the release build.  This is done with the override command.  IF using PR builds to queue up versions then ensure that the -NO switch is passed to the CI versioning element.  

```yaml
- task: PowerShell@2
  displayName: 'PR Versioning'
  condition: eq(variables['Build.Reason'],'PullRequest')
  inputs:
      targetType: 'inline'
      script: |
            # Versioning Powershell - Queue Next Increment.
            c:\Files\BuildTools\PliskyVersioner\Versonify.exe Override -VS=c:\Files\BuildTools\VersionStore\pliskydiagnostics.vstore -Q=..+.0
```

Note - you may need to be careful here of version numbers.  If your CI build increments the build digit and your PR increments the minor digit you would be ok, but if they both operate on the same digit then once an override is queued you need to take care that your CI build doesn't overtake your release build.

## Versioning Pages.

[Home](version-index.md) | [Quick Start](version-quickstart.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)






