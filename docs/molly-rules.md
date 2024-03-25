# Mollycoddle Rules

Mollycoddle rules are highly configurable but there are a default set that ship.  It is expected that not all of these will match your coding style but then you can change the configuration to suit your own structure.

Note that there are gaps between the numbers to allow for other to be specified, for example if your chosen programming lanugage is py and not c# then creating an alternative ruleset to forbid other filetypes should be simple and can fit in the same rule gap if you wish.


### Simplify Code Structure

The following rules help simplify code structure, minimizing mixing up different things in the same location.


#### <a name="MC0001"></a> MC0001 - C# File Extensions Only.

Only csharp related files are permitted in the project structure.

* *.vb/vbproj/java/py are banned at any point in the structure.


#### < name="MC0002"></a> MC0002 - Lazy File Denylist.

Identifies a common set of lazy mistakes with files.

* File can not be called class1.cs
* File can not end in - Copy
* File can not end in (1) or (2)

### MC005X - Getting Started Quickly



#### MC0050 - Readme File Must Exist


A Readme File mus be present in the root of the repo structure.

* /readme.md must exist.




### MC006X - Expected Directories

The following rules help a consistent directory structure at the top level so that it is easy to navigate around a freshly cloned repository and much simpler to get up and running with a new code base.


####  <a name="MC0060"></a> MC0060 - Source code must live in \src


Only a well known directory structure is permitted at the root level of the repository, this must include \src for the source code.

* root\src must exist
* root\src\src must not exist



####  <a name="MC0065"></a> MC0065 - Permitted Root Folders Only

Only a well known directory structure is permitted at the root level of the repository, only the following folders are permitted at the root level.

* root\src
* root\build
* root\doc
* root\res
* root\test

####  <a name="MC0067"></a> MC0067 - Permitted Root Files Only

The root of a repository may only contain the readme, a gitignore and gitattributes files alongside folders.  

```text
root\readme.md   :: Pass
root\build.yml   :: Fail
root\code.cs     :: Fail
root\.gitignore     :: Pass
root\.gitattributes  :: Pass
```

####  <a name="MC0070"></a> MC0070 - Solutions and Projects must live at a consistent level.

Solutions and projects must live at the same level of the folder hierarchy.  Solutions must be in the root of a folder that starts with src and csproj files must be under a rooted src folder and one folder level below.  E.g. \\src\Project\project.csproj

```text
 src\*.sln    :: Pass
 src_*\*.sln    :: Pass
 src\*\\*.csproj   :: Pass
 src\path\path\file.csproj :: Violation
 bonkers\path\file.csproj  :: Violation
```

Additional option allows for "frameworkversions" folder to group up multitargetting solutions.  Where you are not using inline multitargetting all csprojs to support different frameworks must be under a folder called "frameworkversions".  Structure below that is undefined.

```text
 src\**\frameworkversions\**\*.csproj    :: Pass
```

### MC01XX - Consistency Across Teams

Using things like common editorconfig files, gitignore files and so on allows for greater consistency among teams and shift left approach to consistency around code formatting and source code best practices.  Its likely that allowing overrides of these files at lower levels in the code base is wise.


#### <a name="MC0100"></a> MC0100 - Editor Config Must Match Master

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.  Editor config ensures that coding style is enforced by the IDE.

* The folder root\src must have a file called .editorconfig present.    
* That file must be identical to the file stored %masterroot%\master.editorconfig    

####  <a name="MC0101"></a> MC0101 - Git Ignore Must Match Master

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.  Gitignore ensures that sensitive files and files that should not be source managed are skipped.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    

####  <a name="MC0102"></a> MC0102 - Nuget.config must match master.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.  Nuget config controls the way that packages are downloaded and should be consistant for an organisation.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    


### MC02XX - Supply Chain Woes

Rules that deal with managing nuget packages specifically but with slots for other package managers. 

#### <a name="MC0200"></a> MC0200 - Banned nuget packages must not be used.

We use preferred packages for consistency and to reduce retraining between solutions.  Once we have a nuget package that does a job other nugets that do similar jobs are not preferred.

Only preferred packages may be used.    
Alternatives are added to the band list.    
Packages which fail security scanning are added to the band list.  

#### <a name="MC0201"></a> MC0201 - Sponsorlink Packages are Prohibited

Different companies have different rules around how software is permitted for use, in some instances sponsorlink approaches broke those rules and therefore specific packages are excluded. 

Moq versions that had sponsorlink included are prohibited.


### MC04XXX - Source Repository AntiPatterns

#### <a name="MC0400"></a> MC0400 - Common Faulting Source Control Files Must Not Exist

There are some files that should not be checked into git, the presence of any of these files immediately after a clean pull from the repository indicates that something is wrong with source control management.


### MC05XXX - Bypass and Filter Rules

####  <a name="MC0500"></a> MC0500 - Archive folder bypass

There is a place where you can dump any old legacy crud that you wish.  root\archive is immune to all other molly checks and is totally bypassed therefore any legacy or nasty breachy code you like can live in the archive folder.    

####  <a name="MC0500"></a> MC0501 - Commmon Folder Bypass

\obj, \ndependout, \.vs, \.git, \bin are all skipped from validation rules.

