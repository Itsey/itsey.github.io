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
              .SetVersionPersistanceValue(settings.VersioningPersistanceTokenPre)
              .SetDebug(true)
              .SetRoot(Solution.Directory)
              .SetQuickValue(QuickVersion)
            );
        }
    });

    // Now run command
    // nuke VersionQuickStep --QuickVersion "1.2.3"
```

### Detailed Walkthrough of adding Pre-Release and Release Versioning Using Nuke.

This will run through a detailed set of steps to add Semver 2.0 compatible pre-release and release versioning to a nuke build used as part of a Nuget package.  This is just an example walk through with one way of doing it.

#### 0 Prepare your repository / local machine.

You will need to have created a nuke build definition.
You will also need to reference the local versioning tools.

If you do not already have a tool manifest then create one:
```cmd
 > dotnet new tool-manifest
The template "Dotnet local tool manifest file" was created successfully.
```

Then add the versonify tool to allow you to create the commands and reference the package
```cmd
 > dotnet tool install plisky.versonify
You can invoke the tool from this directory using the following commands: 'dotnet tool run versonify' or 'dotnet versonify'.
Tool 'plisky.versonify' (version '1.0.1') was successfully installed. Entry is added to the manifest file X:\Code\ghub\mollycoddle\src\.config\dotnet-tools.json.
```

Add the versonify tool to nuke so that it can find it as a package download.

```cmd
 > nuke :add-package Plisky.Versonify --version 1.0.1
NUKE Global Tool 🌐 version 9.0.3 (Windows,.NETCoreApp,Version=v8.0)
Installing Plisky.Versonify/1.0.1 to X:\Code\ghub\mollycoddle\src\mollycoddle.build\mollycoddle.build.csproj ...
[INF] > "C:\Program Files\dotnet\dotnet.exe" restore X:\Code\ghub\mollycoddle\src\mollycoddle.build\mollycoddle.build.csproj
Done installing Plisky.Versonify/1.0.1 to X:\Code\ghub\mollycoddle\src\mollycoddle.build\mollycoddle.build.csproj
```

#### 1 Create The Version Files.

First we need to create the version files, this can be done with the command line.  This walkthrough will use a nexus url that has partially been configured using an environment variable.

```cmd
versonify  '-Command=CreateVersion' '-VS=%NEXUSCONFIG%[R::plisky[L::https://mynexus.com/repository/plisky/vstore/new-pre.vstore' '-Q=1.0.0.0.0.0' '-Release=Demon'
versonify  '-Command=CreateVersion' '-VS=%NEXUSCONFIG%[R::plisky[L::https://mynexus.com/repository/plisky/vstore/new.vstore' '-Q=1.0.0.0'
```

The command creates a new versioning file using 6 digits for the pre-release version.  This will allow three for our SemVer 2 compatible Major.Minor.Build and then a digit for our pre-release marker, followed by two digits for pre-release increments.

For example:  1.0.0-prerelease.1.0     or  1.0.1-pre-0.1

Then we also create a release version file.  This way the two types of release can increment independently

The output from creating the file looks like this:
```txt
 > versonify  '-Command=CreateVersion' '-VS=%NEXUSCONFIG%[R::plisky[L::mynexus.com/repository/plisky/vstore/molly-pre.vstore' '-Q=1.0.0.0.1.5'
💖 Versioning By Versonify 💖 (1.0.0.0).
Performing Versioning Actions
Using Value From Command Line: 1.0.0.0.1.5
Creating New Version Store: 1.0.0.0.1.5
Saving 1.0.0.0.1.5
```

#### 2 update the version numbers.

Next we will update the version numbers to your chosen naming and increment approach.  This currently involves editing the files directly.  We can set the increment behaviour through the command line but the pre-release value must be edited directly.  

Opening the pre-release file we see this contents:

```txt
"Digits": [
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "1",
            "PreFix": ""
        },
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "0",
            "PreFix": "."
        },
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "0",
            "PreFix": "."
        },
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "0",
            "PreFix": "."
        },
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "1",
            "PreFix": "."
        },
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "5",
            "PreFix": "."
        }
    ],
    
Changing the fourth digit to this - altering the value and the prefix
        {
            "Behaviour": 0,
            "IncrementOverride": null,
            "Value": "prerelease",
            "PreFix": "0"
        },
```

Once we have specified the -prerelease identifier as the fourth digit we have a method of identifying pre-release version numbers that is SemVer 2 compatible.


#### 3 Add automatic increment to the version numbers.

Two of the digits should automatically increment to ensure that we do not get a duplicate version number.  The final digit in the pre-release version number and the third digit in the release version number.  We can update this using the command line.


```cmd
> versonify '-Command=Behaviour' '-VS=%NEXUSCONFIG%[R::plisky[L::https://mynexus.com/repository/plisky/vstore/molly.vstore' '-dg=2' '-Q=AutoIncrementWithResetAny'
💖 Versioning By Versonify 💖 (1.0.0.0).
Performing Versioning Actions
Setting Behaviour for Digit[2] to AutoIncrementWithResetAny(5)
Saving Updated Behaviour

 > versonify '-Command=Behaviour' '-VS=%NEXUSCONFIG%[R::plisky[L::https://mynexus.com/repository/plisky/vstore/molly-pre.vstore' '-dg=5' '-Q=AutoIncrementWithResetAny'
💖 Versioning By Versonify 💖 (1.0.0.0).
Performing Versioning Actions
Setting Behaviour for Digit[5] to AutoIncrementWithResetAny(5)
Saving Updated Behaviour
```

The two updates set the digits to AutoIncrementWithResetAny.  Note that the behaviour command is 0 offset for the digit position so this updates the third digit for the release version and the final digit for the pre-release version.

#### Add the nuke script to version correctly.

Add the references to your two version stores.  One for the pre-release version and one for the release version.
```cs
  settings = new LocalBuildConfig {
    VersioningPersistanceTokenPre = @"%NEXUSCONFIG%[R::plisky[L::mynexus.com/repository/plisky/vstore/molly-pre.vstore",
    VersioningPersistanceTokenRelease = @"%NEXUSCONFIG%[R::plisky[L::https://mynexus.com/repository/plisky/vstore/molly.vstore",
               };
```

It is also useful to have two parameters so that you can configure the versioning behaviour without editing the build script directly.

```cs
[Parameter("Specifies a quick version command for the versioning quick step", Name = "QuickVersion")]
readonly string QuickVersion = "";

[Parameter("PreRelease will only release a pre-release verison of the package.  Uses pre-release versioning.")]
readonly bool PreRelease = true;
```


#### Add the versioning.

The versioning needs to be added prior to the compile step.  

Due to some current issues this step contains more code than will be needed as it contains a workaround for a bug in Versonify.  Essentially if it is a pre-release build then the versioning takes place as usual.  If it is a release build then the release file versioning takes over and queues an update for the next pre-release build to ensure that the pre-release versioning keeps up with the release versioning.

While this seems longwinded assuming that you have your versioning globs in /automation/autoversion.txt then you can just copy and paste the code.

Note that this code sets DryRunMode to true for local builds so the version number will not increment when running locally.

```cs

 public string FullVersionNumber { get; set; } = string.Empty;

 public Target ApplyVersion => _ => _
   .After(ConstructStep)
   .DependsOn(Initialise)
   .Before(Compile)
   .Executes(() => {
       if (settings == null) {
           Log.Error("Build>ApplyVersion>Settings is null.");
           throw new InvalidOperationException("The settings must be set");
       }

       if (Solution == null) {
           Log.Error("Build>ApplyVersion>Solution is null.");
           throw new InvalidOperationException("The solution must be set");
       }

       bool dryRunMode = false;

       if (IsLocalBuild) {
           // Passive Get Current Version
           Log.Information("Local Build - Versioning Set To Dry Run");
           dryRunMode = true;
       }

       string versioningType = "Pre-Release";
       string vtFile = settings.VersioningPersistanceTokenPre;
       if (!PreRelease) {
           vtFile = settings.VersioningPersistanceTokenPreRelease;
           versioningType = "Release";
       }

       Log.Information($"[Versioning]{versioningType} versioning displaying curent version number.");

       var vc = new VersonifyTasks();
       vc.PassiveCommand(s => s
           .SetVersionPersistanceValue(vtFile)
           .SetOutputStyle("azdo")
           .SetRoot(Solution.Directory)
       );

       var mmPath = settings.DependenciesDirectory / "automation";
       mmPath /= "autoversion.txt";

       vc.FileUpdateCommand(s => s
           .SetVersionPersistanceValue(vtFile)
           .AddMultimatchFile(mmPath)
           .PerformIncrement(true)
           .SetOutputStyle("azdo-nf")
           .AsDryRun(dryRunMode)
           .SetRoot(Solution.Directory)
       );

       // BUG.  vc.VersionLiteral is not set to the FileUpdateCommand output so have queued another passive to fix this.

       vc.PassiveCommand(s => s
          .SetVersionPersistanceValue(vtFile)
          .SetOutputStyle("azdo")
          .SetRoot(Solution.Directory)
      );

       Log.Information($"[Versioning]{versioningType} Increment and Update Exsiting Files.({vc.VersionLiteral})");

       if (!PreRelease) {
           // Hack.  Curently the return from versonify is not set to be different display types, we need 3 digit for semver so hacking the last digit off.

           int periodCount = 0;
           string verNumberToUse = string.Empty;
           foreach (char c in vc.VersionLiteral) {
               if (c == '.') {
                   if (periodCount == 2) {
                       break;
                   }
                   periodCount++;
               }

               verNumberToUse += c;
           }

           while (periodCount < 2) {
               verNumberToUse += ".0";
               periodCount++;
           }

           b.Assert.True(verNumberToUse.Count(x => x == '.') == 2, $"The version number ({verNumberToUse}) should be in the format N.N.N.");
           Log.Information($"[Versioning]{versioningType} Applying release version number to pre-release data. ({vc.VersionLiteral} > {verNumberToUse})");

           vc.OverrideCommand(s => s
               .SetVersionPersistanceValue(settings.VersioningPersistanceTokenPre)
               .SetOutputStyle("azdo-nf")
               .AsDryRun(dryRunMode)
               .SetRoot(Solution.Directory)
               .SetQuickValue(verNumberToUse)
           );
       }

       FullVersionNumber = vc.VersionLiteral;
       Log.Information($"[Versioning]Version applied:{vc.VersionLiteral}");
   });
```


#### Test your versioning.

You can specify whether a pre-release version should be used or not using the parameter you created.  Remember that by default your local builds will be in dry run mode so the versioning will not increment in the store.
```cmd
nuke applyVersion --preRelease true 
nuke applyVersion --preRelease false
```