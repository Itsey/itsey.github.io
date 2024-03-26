## Versioning Quick Start for Nuke

### Step 1 
**Create** the storage file which contains the versioning number that you are going to use.  The new file will default to 0.0.0.0 and a fixed behaviour scheme.  This is best done using the command line and placed on a file share.

```dos
pliskytool.exe -Command=CreateVersion -VersionSource=C:\temp\myappname.vstore
```

### Step 2 
**Configure** your source repository with a text file describing which files you want to apply versioning to.  This will contain a list of version minmatches.  

Create a file like one below and save it as autoversion.txt in your repository.This is a set of minmatchers and you should match your code (for example the convention below has source code in a /src folder)

```plaintext
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetFile
**/src/**/CommonAssembly*.cs|NetInformational
**/src/**/*.csproj|StdAssembly
**/src/**/*.csproj|StdInformational
**/src/**/*.txt|TextFile
```

### Step 3 
**Increment** the version number and apply the changes to your source files.  Using Nuke this typically involves a target or adding stages to an existing target.   Typically passive is used to retrieve a version number and the PerformFileUpdate is used to set the version number.

This code depends on a Solution property that is attributed as a solution for Nuke.

```csharp

    [Solution]
    readonly Solution Solution;

Target VersionSource => _ => _
    .Before(Compile)
    .Executes(() => {

        const string versionStorePath = @"D:\Scratch\_build\vstore\versonify-version.vstore";

        VersonifyTasks.PassiveExecute(s => s
          .SetRoot(Solution.Directory)
          .SetVersionPersistanceValue(versionStorePath)
          .SetDebug(true));

        if (IsLocalBuild) {
            Logger.Info("Local build, skipping versioning");
            return;
        }

        VersonifyTasks.IncrementAndUpdateFiles(s => s
         .SetRoot(Solution.Directory)
         .AddMultimatchFile($"{Solution.Directory}\\_Dependencies\\Automation\\AutoVersion.txt")
         .PerformIncrement(true)
         .SetVersionPersistanceValue(VersionPersistancePath));

    });
    

```

### Thats it

With Nuke you can now run the build locally which will allow you to test the configuration.  Obviously doing this for testing you should remove the IsLocalBuild check and potentially add the DryRun check.  DryRun allows you to see what would happen without actually making the changes.

[See more info about using Nuke and Versonify.](version-usingnuke.md)