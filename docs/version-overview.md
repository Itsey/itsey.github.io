## Versioning Pages Navigation.

[Home](version-index.md) |[Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## Versioning Overview

To get up and running really quickly but without much information there is the [Quick Start](version-quickstart.md) guide.  The overview provides more detailed information and the reference provides reference information for when you are using the versioning tool.

Versonify consists of  two parts.  The command line tool and a version store.  The version store is a persisted store of the versioning information that is manipulated via Versonify.  The  version store  is referenced by a single "token" which is either a path to a file on disk or a string with tokens in it to refer to another store ( for example a Nexus repository).

The version store is a json text file and can be edited directly or set with commands available in the command line tool.

### Command Line Tool

Versioning can be referenced from the assembly in your own code or through the command line tool.  The command line tool is designed to allow you to automate versioning tasks and be included in pipelines and DevOps automation.  

The command line tool is versonify.exe and is used for most operations.  Versonify is available as a dotnet tool, to install run `dotnet tool install --local Plisky.Versonify`.`

See the [Command Line Reference](version-commandline.md) for full syntax and information.

### Snippets

These are common snippets used to version on a CI and Release Pipeline.  Items in [] with capitals should be replaced by your own values.

#### CI Versioning Snippet in Azure DevOps.

Assumes that Versonify is in [YOURPATH] and that you are using a file for the matches called AutoVersion.txt

```yaml
- task: PowerShell@2
  displayName: 'CI Versioning'
  condition: not(eq(variables['Build.Reason'],'PullRequest'))
  inputs:
      targetType: 'inline'
      script: |
            # Versioning Powershell.
            [YOURPATH]Versonify.exe UpdateFiles -Root=$(build.sourcesDirectory)\src\ -NoOverride -v=[YOURPATH]\[YOURFILENAME].vstore -Increment -m=$(build.sourcesDirectory)\[YOURPATH]\AutoVersion.txt
            [YOURPATH]Versonify.exe Passive -v=[YOURPATH]\[YOURFILENAME].vstore -O=file
            $storedVersion = Get-Content pver-latest.txt

            Write-Host "Build Version Is: $storedVersion"
            Write-Host "##vso[task.setvariable variable=buildVersionNumber;]$storedVersion"
```

#### PR Build Queued Increment.

The PR build can be used to queue the next version number that will be used on the release build.  This is done with the override command.  If using PR builds to queue up versions then ensure that the -NoOverride switch is passed to the CI versioning element.  

```yaml
- task: PowerShell@2
  displayName: 'PR Versioning'
  condition: eq(variables['Build.Reason'],'PullRequest')
  inputs:
      targetType: 'inline'
      script: |
            # Versioning Powershell - Queue Next Increment.
            [YOURPATH]Versonify.exe Override -v=[YOURPATH]\[YOURFILENAME].vstore -Q=..+.0
```

Note - you may need to be careful here of version numbers.  If your CI build increments the build digit and your PR increments the minor digit you would be ok, but if they both operate on the same digit then once an override is queued you need to take care that your CI build doesn't overtake your release build.
