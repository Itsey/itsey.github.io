# Incrementally Build a Nuke Build

[meta-title]: #Information;#Level100;

### Perquisites

* Solution with Nuke Build
* Solution Compiles

### Steps

1 - Navigate to the solution location

```cmd
cd c:\files\code\<path>\src
```

[meta]: Step;#Beginner;

#### 20 - Review the key Steps

[meta]:Explanation;

The key steps are default steps that are present in every build, by convention.  This means that once these steps are known all builds will follow the same conventions. Additional steps are also posible.
[meta]:

Initialise
PreflightStep
PrepareStep
AssembleStep
ValidateStep
PackageStep
DeployStep

Show the step tree with the command

```cmd
nuke --plan
```

[meta]:Step;

#### 30 - The Initialise Step

```cmd
nuke Initialise
```

Initialisation, placed in Build.cs

Initialisastion contains variables and paths and basic configurable setup that can be stored for the rest of the script. Build logic is here just initialisation variables.

### Step - 32 - Preflight

Preflight checks take place here, fast feedback on standard issues and conventions to make sure that the build can give feedback as fast as possible if something is wrong.

40 - Failed MC Rules?

From the Src Folder

```cmd
copy C:\Files\OneDrive\Dev\PrimaryFiles\master.editorconfig .\.editorconfig
```

```cmd
copy C:\Files\OneDrive\Dev\PrimaryFiles\master.nuget.config .\nuget.config
```

From the root folder

```cmd
copy C:\Files\OneDrive\Dev\PrimaryFiles\master.gitignore .\.gitignore
```

Permitted root folders include
src, build, test, doc, res, src*, data, tools

#### 35 - Prepare Step

```cmd
nuke PrepareStep
```

#### 45 - AssembleStep.

```cmd
nuke AssembleStep
```

This does the compilation, quite often useful to pass --configuration Release

#### 50 - ValidateStep.

```cmd
nuke ValidateStep
```

[meta]:Explanation;

The validation step occurs post build, it includes quality assessments like unit tests and sonarqube scanning.  It should be used to validate that the build has completed successfully and give fast feedback to the engineer prior to deployment.

### 52 - PackageStep

```cmd
nuke PackageStep
```

#### 55 - ReleaseStep.

```cmd
nuke DeployStep
```

[meta]:Explanation;

The DeployStep deploys the code physically to an environment.  For development CI pipelines this will be to the first CD development environment.  
