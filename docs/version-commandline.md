## Versioning Pages Navigation.

[Home](version-index.md) |[Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## Versonify Command Line reference

### Command Line Options

Command line options are prefixed with -.  They are postfixed with =.  
e.g. -Command=CreateVersion 

Commands can also be passed positionally as the first argument.  For example `versonify.exe UpdateFiles ...` is equivalent to `versonify.exe -Command=UpdateFiles ...`.

```plaintext
-Command                    Specify the Command that is to be run
-VersionSource  (-v)       Specify an initialisation string to a supported version source
-Increment  (-I)            Increment the version number during the command operation.
-Digits  (-D)               Provide a value for the index of the digits to be displayed or ammended
-QuickValue  (-Q)           Provide a value for the versioning command.
-MinMatch  (-m)            Provide a file or list of minmatches to identify files to update.
-Root                       The root folder to recursively search for files to update.
-DryRun                     If specified then no updates are made, but output is written to the logs.
-Output  (-O)               Specifies output options to write the version number somewhere. Supports Env,File,Con,AzDo,Vsts and optional -nf suffix for Nuke Fusion output
-NoOverride                 Specifies that overrides should be ignored.  The older `-NO` alias is deprecated.
-Release  (-R)              Specifies a release name to be used in the version number.  This is primarily used for release versions and is not normally used for build versions.

-Debug                      Enables trace handling for debugging and additional logging.
-Trace                      Enables level of trace  (set to Info,Verbose,Off)


Full Example Commandline:

versonify.exe UpdateFiles -Root=c:\src\ -VersionSource=c:\store\pversioner.vstore -Increment -MinMatch="**/*.csproj|StdFile;**/*.csproj|StdAssembly;**/*.csproj|StdInformational"
```

### Commands

#### Create Version

Creates a new default version number 

```plaintext
-Command=CreateVersion

Requires:
-VersionSource  (-v)
```

```dos
versonify.exe -Command=CreateVersion -VersionSource=C:\temp\aversion.vstore
```

Will create a new version at 0.0.0.0 in the source specified by version source, this will be persisted with the default values of Fixed versioning. Therefore your version number will default to 0.0.0.0.
To create a version number with a specific value use the -QuickValue option.

```dos
versonify.exe -Command=CreateVersion -VersionSource=C:\temp\aversion.vstore  -Q="1.0.0.0" -Release=exampleRelease
```

#### Passive

Passively reads the version number for use in scripts.

```plaintext
-Command=Passive

Requires:
-VersionSource  (-v)
```

```dos
versonify.exe -Command=Passive -VersionSource=C:\temp\aversion.vstore
versonify.exe -Command=Passive -VersionSource=C:\temp\aversion.vstore -O=File
```

Will load the version number into the tool then perform no action.  This is only really used in conjuction with the -O output option to ensure that the version number is made available to a calling or alternative process.

If `-Release` is also specified with the Passive command then Versonify will output the stored release name rather than the full version number.

##### Output options

Output options are specified to determine where the output should be written.

```plaintext
-O=<outputdestination>
-O=<outputdestination>:<option>
-O=<outputdestination>-nf
-O=<outputdestination>:<option>-nf
```

Output Destinations can be one of the following.

* env - Writes to an environment variable PVER-LATEST.  If `-Release` is specified then PVER-RELEASE is used instead.
* con - Writes to the console.
* file - Writes to a file. This defaults to pver-latest.txt in the current directory. If `-Release` is specified then the default file name is pver-release.txt. Option can specify an alternative file name.
* azdo - Writes an Azure Pipelines formatted string to the console.  Option can specify a variable name.
* vsts - Alias of `azdo`.

Add the `-nf` suffix to also emit the Plisky.Nuke.Fusion compatibility markers to the console output.  Typical values are `con-nf` and `azdo-nf`.

When the Azure Pipelines output is selected the string written is in the form

```plaintext
"##vso[task.setvariable variable=<variablename>;isOutput=true]<versionnumber>"
```

This will set the variable in variablename to have the value of the version number.  The default variable name is  CodeVersionNumber.  To replace this with your own variable specify the variable name after a colon in the output command.

```dos
Command > versonify.exe Passive -VersionSource=C:\temp\aversion.vstore -O=azdo
Output  > "##vso[task.setvariable variable=CodeVersionNumber;isOutput=true]1.2.3.4"

Command > versonify.exe Passive -VersionSource=C:\temp\aversion.vstore -O=azdo:version
Output  > "##vso[task.setvariable variable=version;isOutput=true]1.2.3.4"
```

When the `-nf` suffix is used Versonify also emits Plisky.Nuke.Fusion compatibility markers such as `PNFV]`, `PNF2]`, `PNF3]`, `PNF4]`, `PNQF]` and `PNFN]`.

#### Override

Overrides the values of version numbers at the point of next increment

```plaintext
-Command=Override

Requires:
-VersionSource  (-v)
-QuickValue (-Q)
```

```dos
versonify.exe -Command=Override -VersionSource=C:\temp\aversion.vstore -Q=+.+.0.0
```

Will create a pending version that will be applied on the next increment.  This will override changes that the default increments will perform applying a pattern.  This is normally used for release versions, where versions do not follow the same pattern as build versions.  

To alter the behaviour specify a new pattern separated by . therefore +.+.+.+ increments each of a four digit version number on the next increment.

\+ = Increment this digit.  
\- = Decrement this digit.  
nnnn = Any number of digits use this as the version number for this digit  
abc  = Any number of letters replaces the digit with this version (for named digits)  

**examples**  
1.0.0.0  =>  +...  => 2.0.0.0  
1.1.1.1  =>  +.+.+.+ => 2.2.2.2  
1.1.1.1 => +.-.+.-  => 2.0.2.0  
1.0.0.0 => +.0.alpha.0  => 2.0.alpha.0  

#### Update Files

Updates the values of version numbers. If the increment option is specified then the version number is incremented first before updating the files.

```plaintext
-Command=UpdateFiles

Requires:
-VersionSource  (-v)
-Root
-MinMatch 

Optional:
-Increment
-DryRun
-MinMatch 
-NoOverride
```

```dos
versonify.exe -Command=UpdateFiles -VersionSource=C:\temp\aversion.vstore -Root=C:\Build\Code\MyApp -MinMatch=AutoVersion.txt
```

Will optionally increment the version number specified by the source and then run through the directory specified by root and update any files that are matched by the minmatchers for the specified file types.  There are a default set of minmatches in effect but they can be overriden.

To override a minmatch specify it using the -MinMatch command.  The older `-MM` alias is deprecated.  This is a series of one or more strings separated by ;.  If a single string is passed with no ; and if this refers to a file that exists on disk then this file will be parsed for MinMatches instead.  The file format is as follows.

It is generally more convenient to specify the file and store it in your source repository than to configure all of the minmatches on the command line using the ; syntax.

```plaintext
<minmatch to the file>|<FileTypeToMatch>
```

Each line in the file adds a new minmatch. 

```plaintext
**/MyApp/commonAssemblyInfo.cs|NetAssembly
**/MyApp/_Dependencies/CDSupport/readme.txt|TextFile
**/MyApp/AppDir/App.csproj|NetInformational
**/MyApp/AppDir/App.csproj|NetFile
**/MyApp/AppDir/App.csproj|Wix
**/_Dependencies/versioning.nuspec|Nuspec
**/MyApp/AppDir/AssemblyInfo.cs|StdAssembly
**/MyApp/AppDir/AssemblyInfo.cs|StdInformational
**/MyApp/AppDir/AssemblyInfo.cs|StdFile
```

The pipe separator separates the minmatch from the type of file that it is updating.  Multiple file types can reside in the same file and therefore use the same minmatch.

Each file type has a rule to determine how to match versions, see [version matching reference](version-vermatchref.md).

Full Example Command Line:

```dos
versonify.exe UpdateFiles -Root=c:\src\ -v=c:\store\pversioner.vstore -Increment -m="**/*.csproj|StdFile;**/*.csproj|StdAssembly;**/*.csproj|StdInformational"
```

This will search the folder c:\src for all .csproj files and attempt to add the .net standard versioning for the three different file types to any csproj files that are
found.  Note that for the std file type it will look inside the file and see whether it looks like a net std file or a framework one.  Framework ones
will not be updated. 

#### Set

Sets the value of a version digit.  This will update the version number in the source specified by the -VersionSource option.

The -Digits option specifies which digit values to set.  This can be a single digit or multiple of digits e.g. -Digits="0;2;3" will set the value of digits 0, 2, and 3.  The * will set the value of all digits. The new digit value is specified using the -QuickValue option. This can only be a single digit.

```plaintext
-Command=Set

Requires:
-VersionSource (-v)
-Digits (-d) and -QuickValue (-Q)    or    -Release

Optional:
-DryRun
```

If the [behaviour](version-reference.md#behaviours) of the digit is Fixed, then the value of the digit can be set to a string. For all other behaviours, the digit value must be set to an integer.
Note: when a digit's behaviour is set to ReleaseName[8], the value of the digit is set to the release name specified in the version source.  It is not possible to set the value of a digit with this behaviour using the -QuickValue option.

This example will set the value of the digit in position [0] to 2.

```dos
versonify.exe -Command=Set -VersionSource=C:\temp\aversion.vstore -Digits=0 -Q=2
```

The complete version number can be set by passing in the version number as the -QuickValue option using the dot '.' as a separator.  This will set all of the digits to the values specified in the version number.

```dos
versonify.exe -Command=Set -VersionSource=C:\temp\aversion.vstore -Q="1.2.3.4"
```

Use the -Release option to set the release name in the version source. 

```dos
versonify.exe -Command=Set -VersionSource=C:\temp\aversion.vstore -Release=MyNewReleaseName
```

#### Behaviour

Passively displays the [behaviour](version-reference.md#behaviours) of a versioning digit(s). 
-QuickValue can be optionally passed to set the behaviour of the digit.

```plaintext
-Command=Behaviour 

Requires:
-VersionSource  (-v) and -Digits (-d)

Optional:
-Output  (-O)
-QuickValue  (-Q)
-DryRun
```

```dos
versonify.exe -Command=Behaviour -VersionSource=C:\temp\aversion.vstore -Digits=*
versonify.exe -Command=Behaviour -VersionSource=C:\temp\aversion.vstore -Digits=1 -O=file
```

This will display the behaviour of the versioning digits in the version source.  The -Digits option specifies which digits to display.  This can be a single digit or multiple of digits e.g. -Digits="0;1;2" will display the behaviours of digits 0, 1, and 2.  The * will display the behaviour of all digits.

If the -QuickValue option is specified then the behaviour of the digit will be set to the value specified.  This can be either the behaviour number or the string that represents the behaviour. The * will set the behaviour of all digits.
Both the following examples will set the behaviour of the first digit (position 0) to Fixed (0).  Note all of the digit offsets are zero based so the first digit is -Digits=0.  

```dos
versonify.exe -Command=Behaviour -VersionSource=C:\temp\aversion.vstore -Digits=0 -Q=Fixed
versonify.exe -Command=Behaviour -VersionSource=C:\temp\aversion.vstore -Digits=0 -Q=0
```

#### Prefix

Sets the prefix for a digit in the version source.

```plaintext
-Command=Prefix

Requires:
-VersionSource  (-v), -Digits (-d) and -QuickValue (-Q)

Optional:
-DryRun
```

```dos
versonify.exe -Command=Prefix -VersionSource=C:\temp\aversion.vstore -Digits=2 -Q="-"
```

This example will set the value of the prefix of digit in position [2] to a dash, "-".

Prefix command supports using the wildcard * to set the prefix for all digits (excluding the digit in position [0]).  For example, the following command will set the prefix of all digits except the first to a dash, "-". To set the value of the first digit prefix specify -Digits=0.  

```dos
versonify.exe -Command=Prefix -VersionSource=C:\temp\aversion.vstore -Digits=* -Q="-"
```

The prefix provided in the -QuickValue can be anything, but for Semantic Versioning (semver) use prefixes of dot(.), dash(-), or plus(+) only.

#### Using No Override

When setting up multiple branches it is sometimes useful to be able to ignore an override when a specific branch is versioned.  To do this specify -NoOverride.  The older `-NO` alias is deprecated.      
The most common scenario here is when the Pull Request build is used to reset the version ready for release.  When using the pull request builds to version then it is possible that a build on the source branch happens after the PR build but before the release branch has run.  This will cause the source branch to incorrectly version.  To avoid this add the -NoOverride switch to the source branch versioning element.

#### Using -Debug

The -debug enables tracing for detailed error investigation.  See [Setting Configuration Resolvers](diags-bilge-configurationResolvers.md) for full details. Typically this is set to v-** for verbose when trying to resolve issues.

#### Using -z

The -z command ensures that non zero exit codes are suppressed.  This is used to prevent build failures on versonify errors, does not work for an incorrectly specified command line.  Added in Austen 1.0.2 which made Versonify error when no updates were made to files. This default makes it simpler for most people to correctly identify when there are files not being updated as expected.

#### Using --QQpnf

This is not deigned to be used by consumers of Versonify.  It returns an exit code indicating the compatibility level of the Versonify command line so that scripts that are calling Versonify can identify which features it supports.  The current return code is 200.   Versions prior to Austen 1.0.2 will not support this argument.
