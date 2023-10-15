### Mollycoddle Quick Start Guide.

#### Get Mollycoddle

Go to the Git Hub Releases Page:  https://github.com/Itsey/mollycoddle/releases
Download the latest release.

There will be three zip files, the mollycoddle binary compiled for Windows x86 and the two rules files zip files.  To start wtih use the default rules.

Extract MollyCoddle_V*.zip to a folder of your choice.    
Extract QuickStartMollyRules.zip to a folder of your choice.    
    
In the examples below c:\molly will be used with the rules being in c:\molly\quickstartrules\.


#### Using Mollycoddle

Using your command prompt / preferred terminal navigate to the location of your code, and then call the full pathname where you extracted mollycoddle above.  

```text
cd\
cd files
cd code
c:\files\code\>
c:\files\code\>c:\molly\mollycoddle.exe
```

This should put out an error suggesting you are unable to run mollycoddle without a rules file.  Mollycoddle requires both a code structure to analyse and a set of rules to use for the analysis.

```text
##[command]MollyCoddle Online
##vso[task.logissue type=error] ⚠ Error: InvalidCommand - Directory Was Not Correct (Does this directory exist? [])
```


Execute MollyCoddle from the command line, passing a parameter of the path to scan and a rules file. For this example we will use the default rules file and assume that the source code is in a folder called sourcecode beneath the current directory:

```text
// Synatx - mollycoddle.exe <RootOfRepository> [-rulesfile="<PathToMollyRulesFile"] 
c:\molly\mollycoddle.exe "sourcecode" -rulesfile=c:\molly\quickstartrules\quickstart.mollyset -formatter=none
```

For this to work you need to have your source code in a location below where you are executing the command from, in a folder called source code.  The mollycoddle binaries need to be placed in C:\molly\ and the rules files extracted to c:\molly in folders.

When you execute the command you should see this


```text
Info: MollyCoddle Online
Alert : 18:56:23 (1)>> mollycoddle Online. v-1.0.0.0. @14/10/2023 18:56:23

😎 Completed.No Violations, Mollycoddle Pass. ( Took 922ms.)
```

or if the src folder does not exist

```text
Info: MollyCoddle Online
💩 Violation: Use src for your source code. ((c:\molly\sourcecode\src) must exist and it does not.)
😢 Completed.Total Violations 1.  ( Took 120ms.)
```

