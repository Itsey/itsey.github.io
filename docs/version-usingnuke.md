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
            const string versionStorePath = @"<PATH>\versonify-version.vstore";

            VersonifyTasks.PassiveExecute(s => s
              .SetRoot(Solution.Directory)
              .SetVersionPersistanceValue(versionStorePath)
              .SetDebug(true));

            VersonifyTasks.PerformFileUpdate(s => s
             .SetRoot(Solution.Directory)
             .AddMultimatchFile($"{Solution.Directory}\\_Dependencies\\Automation\\AutoVersion.txt")
             .PerformIncrement(true)
             .SetVersionPersistanceValue(VersionPersistancePath)
             //.SetDebug(true)     Want more debugging info?
             //.AsDryRun(true)     Want to see what would happen without doing it?
             //.SetRelease("")     Using release names?         
             );
        });
```

