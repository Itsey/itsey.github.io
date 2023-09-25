### Mollycoddle Command Line Options


The command line options for Molly are as follows:

```text
❯ .\mollycoddle.exe <PATH> -masterRoot=<PATH> -formatter=<plain|azdo> -warnonly -addrulehelp  -disabled -debug=<DEBUG> 
```

### -dir  (or default single parameter)

The directory to scan, mollycoddle should be run from the parent directory e.g. mollycoddle .\codetoscan  that will give the most reliable results.

### -primaryRoot

This holds the primary root path for all of the files to be compared against their primary files.  Pass a full path to the root where the primary files live.  Therefore if you have a master.editorconfig 

### -formatter

This determines the format output for messages written to the console.  Options are:

plain - Writes standard format text.    
azdo - Writes text formatted for azure devops pipelines

```text
Azdo Output:
##vso[task.logissue type=error]Error: Error - Unable To Read RulesFiles

Plain Output:
Error: Error - Unable To Read RulesFiles
````

### -warnonly

Performs the full mollycoddle checks but returns zero upon completion. 

### -addrulehelp

Includs additional help links in the output to support simpler diagnostics

### -disabled

Disables all checks and returns zero

### -debug

Allows the specification of a debug string to write additional in depth debugging information.  Use v-** for verbose.
