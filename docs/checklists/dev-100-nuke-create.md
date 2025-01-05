# Create a new Nuke Build.

### Perquisites

* Solution created
* Solution Compiles

### Steps

#### 10 - Navigate to the solution location

```csharp
cd c:\files\code\path\src
nuke setup --root .
```

[step]:

#### 20 - Name The Build

Name the build [solutionName.build] place it in the path .\src\[solutionname.build] use [nuke8].

[step]:

#### 30 - Reload solution and recompile.

The nuke setup modifies the solution file so VS will prompt to reload when you return to it.

#### 32 - Add Nuget Packages

```text
Nuget.CommandLine
Plisky.Diagnostics
Plisky.Listeners
Plisky.Mollycoddle
Plisky.Nuke.Fusion
Plisky.Versionify
```

#### 35 - Update Build.cs

Remove comments, addd the following

```csharp
partial class Build : NukeBuild {

    public static int Main() => Execute<Build>(x => x.Compile);
    [Parameter("Configuration to build - Default is 'Debug' (local) or 'Release' (server)")]
    readonly Configuration Configuration = IsLocalBuild ? Configuration.Debug : Configuration.Release;
    [GitRepository]
    readonly GitRepository GitRepository;
    [Solution]
    readonly Solution Solution;
    AbsolutePath SourceDirectory => RootDirectory / "src";
    //AbsolutePath ArtifactsDirectory => Path.GetTempPath() + "\\artifacts";
    AbsolutePath ArtifactsDirectory => @"D:\Scratch\_build\vsfbld\";

    LocalBuildConfig settings;
```

[step]:

#### 40 - Add a new folder called "steps" copy the standard steps.

```cmd
cd steps
```

```cmd
copy C:\files\OneDrive\Dev\Templates\src\nuke-default-steps\*.* .
```

50 - WARNING:  

```csharp
Hardcoded incorrect values
    AbsolutePath ArtifactsDirectory => @"D:\Scratch\_build\vsfbld\";
```

[step]:

#### 60 - Add the initialise step

```csharp
Target Initialise => _ => _
 .Before(PrepareStep)
 .Executes(() => {

     if (Solution == null) {
         Logger.Error("Solution is null");
         throw new InvalidOperationException("The solution must be set");
     }


     settings = new LocalBuildConfig();
     settings.ArtifactsDirectory = @"D:\Scratch\_build\vsfbld\";
     settings.NonDestructive = false;
     settings.VersioningPersistanceToken = @"D:\Scratch\_build\vstore\plisky-plumbing.vstore";
     settings.MainProjectName = "Plisky.Plumbing";


     if (settings.NonDestructive) {
         Logger.Info("Initialised - In Non Destructive Mode.");
     } else {
         Logger.Info("Initialised - In Destructive Mode.");
     }

 });
```

[step]:

#### 70 - Compile and execute build

```cmd
cd ..
.\build.ps1
```

80 - Finished, Update Checklists.