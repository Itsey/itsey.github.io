## Versioning Pages Navigation.

[Home](version-index.md) |[Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## Versonify Nuke Reference

Nuke is a build system, if you are using nuke this page should help you integrate.  If you are not then this will not be of any relevance to you.

#### Plisky.Nuke.Fusion

The support for plisky tools in Nuke is through the Plisky.Nuke.Fusion package as well as the individual packages that are required for each tool.  First install the fusion package.

### Specifying a separate version target.

You can place the versioning in its own target or inline with something like the compile target.  Typically you will not want to use versioning during local builds.  Your versioning will need to run before your compile target to ensure that version number increments are included in the compiled code.

Depending on your version store you will need to specify an initialization string, for the default this is a disk path or SMB share.

```csharp
    Target VersionSource => _ => _
        .Executes(() => {

            if (IsLocalBuild) {
                Logger.Info("Local build, skipping versioning");
                return;
            }

            // Note Nexus Paths or SMB shares also possible
            const string versionStorePath = @"<PATH>\versonify-version.vstore";

            var vc = new VersonifyTasks();
            vc.PassiveCommand(s => s
              .SetRoot(Solution.Directory)
              .SetVersionPersistanceValue(versionStorePath)
              .SetDebug(true));

            vc.FileUpdateCommand(s => s
             .SetRoot(Solution.Directory)
             .AddMultimatchFile($"{Solution.Directory}\\_Dependencies\\Automation\\AutoVersion.txt")
             .PerformIncrement(true)
             .SetVersionPersistanceValue(versionStorePath)
             //.SetDebug(true)     Want more debugging info?
             //.AsDryRun(true)     Want to see what would happen without doing it?
             //.SetRelease("")     Using release names?         
             );
        });
```

### Using Nuke To Control Quick Versions

By creating a target that is not part of the pipeline but can be called manually you can update the version number quite quickly.   This allows you to change major verison numbers for releases on the next time the release runs.

```csharp
// Add Parameter to Nuke
[Parameter("Specifies a quick version command for the versioning quick step")]

readonly string QuickVersion = "";

// Target Unused in pipeline
public Target VersionQuickStep => _ => _
    .After(ConstructStep)
    .DependsOn(Initialise)
    .Before(Compile)
    .Executes(() => {
        Log.Information($"Manual Quick Step QV:{QuickVersion}");

        if (!string.IsNullOrEmpty(QuickVersion)) {
            var vc = new VersonifyTasks();
            vc.OverrideCommand(s => s
              .SetVersionPersistanceValue(settings.VersioningPersistanceToken)
              .SetDebug(true)
              .SetRoot(Solution.Directory)
              .SetQuickValue(QuickVersion)
            );
        }
    });

    // Now run command
    // nuke VersionQuickStep --QuickVersion "1.2.3"
```
