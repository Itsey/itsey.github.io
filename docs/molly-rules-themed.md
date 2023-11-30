# Mollycoddle Rules - Themed

When Mollycoddle first was made it was an internal tool with themed rule names, this ruleset still exists and is documented here.  Many people prefer the much more plain speaking rules which are now the default set.


#### <a name="MC0001"></a> MC0001 - One Language To Rule Them All.

Only csharp related files are permitted in the project structure.

* *.vb/vbproj/java/py are banned at any point in the structure.


#### <a name="MC0050"></a> MC0050 - The Pen is mightier than the compiler.

A Readme File mus be present in the root of the repo structure.

* /readme.md must exist.



#### MC0060 - Hot Source Must Live In Hot Src.

Only a well known directory structure is permitted at the root level of the repository, this must include \src for the source code.

* root\src must exist
* root\src\src must not exist



#### MC0065 - Good Code has Good Roots.

Only a well known directory structure is permitted at the root level of the repository, only the following folders are permitted at the root level.

* root\src
* root\build
* root\doc
* root\res
* root\test

#### MC0067 - Dirty Roots Grow Dirty Things.

The root of a repository may only contain the readme, a gitignore and gitattributes files alongside folders.  

```text
root\readme.md   :: Pass
root\build.yml   :: Fail
root\code.cs     :: Fail
root\.gitignore     :: Pass
root\.gitattributes  :: Pass
```

#### MC0070 - Everything, precisely, where it should be.

Solutions and projects must live at the same level of the folder hierachy.  Solutions must be in the roolt of a folder that starts with src and csproj files must be under a rooted src folder and one folder level below.  E.g. \\src\Project\project.csproj

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



#### <a name="MC0100"></a> MC0100 - All your editor config are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The folder root\src must have a file called .editorconfig present.    
* That file must be identical to the file stored %masterroot%\master.editorconfig    

####  <a name="MC0101"></a> MC0101 - All your gitignore are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    

####  <a name="MC0102"></a> MC0102 - All your nuget.config are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    



#### <a name="MC0200"></a> MC0200 - No Naughty Nugets Needed.

We use preferred packages for consistency and to reduce retraining between solutions.  Once we have a nuget package that does a job other nugets that do similar jobs are not preferred.

Only preferred packages may be used.    
Alternatives are added to the band list.    
Packages which fail security scanning are added to the band list.  

#### <a name="MC0201"></a> MC0201 - Making A Moquery Out Of OSS.

We have very specific rules around packages that talk outside of the development environment which requires an entirely different governance process.   Until this compliance process has been completed for the sponsorlink addition to moq we are unable to use it.

Moq versions that had sponsorlink included are prohibited.


#### <a name="MC0400"></a> MC0400 - Git the heck outa here.

There are some files that should not be checked into git, the presence of any of these files immediately after a clean pull from the repository indicates that something is wrong with source control management.



####  <a name="MC0500"></a> MC0500 - Archive Is As Archive Does.

There is a place where you can dump any old legacy crud that you wish.  root\archive is immune to all other molly checks and is totally bypassed therefore any legacy or nasty breachy code you like can live in the archive folder.    