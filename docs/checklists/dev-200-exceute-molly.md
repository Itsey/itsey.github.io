# Execute Molly Coddle Rules

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

Initialise
PreflightStep
PrepareStep
AssembleStep
ValidateStep
PackageStep
DeployStep

[meta]:Step;

#### 30 - The Initialise Step

```cmd
nuke Initialise
```

Initialisation, placed in Build.cs

#### 35 - Prepare Step

```cmd
nuke PrepareStep
```

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

#### 45 - AssembleStep.

```cmd
nuke AssembleStep
```

#### 50 - ValidateStep.

```cmd
nuke ValidateStep
```

#### 55 - ReleaseStep.

```cmd
nuke ReleaseStep
```
