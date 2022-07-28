### Creating MollyCoddle Rules

MollyCoddle rules are a json format which describes a set of parameters to one of the installed checkers in mollycoddle.  Mollycoddle has three active checkers at the moment.

| Checker               |Role| Commands          |
|-----------------------|-----|-------------------|
| FileValidationChecker |Checks for files, locations and contents| MustExist,<br>MustNotExist,<br>MatchWithMaster,<br>IfExistMustBeHere |
|DirectoryValidationChecker|Checks for directories and their locations| MustNotExist,<br/>MustExist,<br/>ProhibitedExcept|
|NugetPackageChecker| Checks for referenced Nuget Packages| ProhibitedPackagesList|

#### Rules file structure

Within the rules file structure the Validators object contains the implementaiton of the rules details.  The control value is specified from the commands list above and below.  The other parameters depend on the control.

Where pattern matches are made the variable %ROOT% can be used to indicate the repository root.

```json
{
    "Rules": [
        {
            "Link": "<A Hyper Link To Rules Help>",
            "Name": "<The descriptive name of your rule>",
            "RuleReference": "<A Unique Reference>",
            "Validators": [
                {
                    "AdditionalData": [
                        "<An Array of one or more strings of supporting info>"
                    ],
                    "Control": "<A keyword indicating the type of rule>",
                    "PatternMatch": "<A minmatch Pattern to match on>",
                    "ValidatorName": "<The type of validator used>"
                }
            ]
        }
    ],
    "RulesetName": "A Name to group all the rules together"
```




#### The File Validation Checker

The file validation checker looks for specific filename matches and can then perform a variety of options.  This can include checking for a banned location, ensuring a file must exist at a location or comparing the contents with another file.

##### MustExist & MustNotExist

Control: "MustExist" or "MustNotExist"
PatternMatch:  A minmatch pattern which either must be matched at least once ( for must exist ) or may never be matched for MustNotExist.    
ValidatorName: FileValidationChecks    

Example: Force a readme.md to be found in the root of the repository
```json
 "Validators": [
                {
                    "AdditionalData": null,
                    "Control": "MustExist",
                    "PatternMatch": "%ROOT%\\readme.md",
                    "ValidatorName": "FileValidationChecks"
                }
            ]
```


##### MatchWithMaster


Control: "MatchWithMaster"    
PatternMatch:  A minmatch pattern which identifies one or more files.  Once the files are found they are compared with the master file.  This master file and the found file must match exactly.  %MASTERROOT% can be used to take a master root path passed into the tool.    
ValidatorName: FileValidationChecks

Example: Everyone has to use the same editorconfig file
```json
 "Validators": [
                {
                    "AdditionalData": [
                        "%MASTERROOT%\\master.editorconfig"
                    ],
                    "Control": "MatchWithMaster",
                    "PatternMatch": "**/.editorconfig",
                    "ValidatorName": "FileValidationChecks"
                }
            ]
```


##### IfExistMustBeHere


Control: "IfExistMustBeHere"
PatternMatch:  A minmatch pattern which matches a specific file type.  Each time that minmatch is matched then the file matching is compared against those in the additional data options.  Additional data supplies a series of other minimatches, one of which must match or it is a violation.    
ValidatorName: FileValidationChecks

Example: csproj files must either be in one folder below src or in a specific folder called frameworkversions.
```json
 "Validators": [
                { 
                    "AdditionalData": [
                        "**/src*/*/*.csproj",
                        "**/src*/**/FrameworkVersions/**/*.csproj"
                    ],
                    "Control": "IfExistMustBeHere",
                    "PatternMatch": "**/*.csproj",
                    "ValidatorName": "FileValidationChecks"
                }
            ]
```




#### The Directory Validation Checker

The directory validation checker looks for specific directory paths and structures.  


##### ProhibitedExcept
Control:ProhibitedExcept    
PatternMatch:  The pattern that is prohibited, if this is matched then this folder will be a violation unless it matches one of the minmatches passed as additional data.   
ValidatorName:DirectoryValidationChecks

Example:  All folders under root are prohibited, except src.
```json
"Validators": [
                {
                    "AdditionalData": [
                        "%ROOT%\\src",
                    ],
                    "Control": "ProhibitedExcept",
                    "PatternMatch": "%ROOT%\\*",
                    "ValidatorName": "DirectoryValidationChecks"
                }
            ]
```

##### MustExist & MustNotExist
Control:MustExist or  MustNotExist
PatternMatch:  The pattern that either must be matched at least once for the solution in the case of must exist or can never be matched in the case of must not exist.
ValidatorName:DirectoryValidationChecks

Example:  Src folder must exist in root, but cant be duplicated.
```json
 "Validators": [
                {
                    "AdditionalData": null,
                    "Control": "MustExist",
                    "PatternMatch": "%ROOT%\\src",
                    "ValidatorName": "DirectoryValidationChecks"
                },
                {
                    "AdditionalData": null,
                    "Control": "MustNotExist",
                    "PatternMatch": "%ROOT%\\src\\src",
                    "ValidatorName": "DirectoryValidationChecks"
                }
            ]

```




#### The Nuget Validation Checker

The nuget validation checker operates on nuget package references.

##### ProhibitedPackagesList
Control:ProhibitedPackagesList    
PatternMatch:  The pattern to match to identify the nuget file, normally this is a csproj file where the packages are referenced.    
AdditionalData: A list of packages that if found will indicate a violation.    
ValidatorName:NugetValidationChecks    

Example:  Banned.package1 and Banned.Package2 can not be used
```json
 "Validators": [
                {
                    "AdditionalData": [
                        "banned.package1",
                        "banned.package2"
                    ],
                    "Control": "ProhibitedPackagesList",
                    "PatternMatch": "**\\*.csproj",
                    "ValidatorName": "NugetValidationChecks"
                }

```

