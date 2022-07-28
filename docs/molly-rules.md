# Mollycoddle Rules

Mollycoddle rules are highly configurable but there are a default set that ship.  It is expected that not all of these will match your coding style but then you can change the configuration to suit your own structure.

Note that there are gaps between the numbers to allow for other to be specified, for example if your chosen programming lanugage is py and not c# then creating an alternative ruleset to forbid other filetypes should be simple and can fit in the same rule gap if you wish.


#### MC0001 - One Language To Rule Them All.

Only csharp related files are permitted in the project structure.

* *.vb/vbproj/java/py are banned at any point in the structure.


#### MC0050 - The Pen is mightier than the compiler.

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



#### MC0100 - All your editor config are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The folder root\src must have a file called .editorconfig present.    
* That file must be identical to the file stored %masterroot%\master.editorconfig    

#### MC0101 - All your git ignore are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    

#### MC0102 - All your nuget.config are belong to us.

Master files ensure that the files in the repos are kept up to date with the most recent changes in style and settings.

* The root folder must have a file called .gitignore present.    
* That file must be identical to the file stored %masterroot%\master.gitignore    



#### MC0200 - No Naughty Nugets Needed.

We use preferred packages for consistency and to reduce retraining between solutions.  Once we have a nuget package that does a job other nugets that do similar jobs are not preferred.

Only preferred packages may be used.    
Alternatives are added to the band list.    
Packages which fail security scannign are added to the band list.    