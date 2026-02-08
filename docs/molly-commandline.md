[Home](index.md) | [Mollycoddle](molly-index.md) | [Command Line](molly-commandline.md) | [QuickStart](molly-quickstart.md) | [CreateRules](molly-createRules.md) |  [Nuke](molly-nuke.md)

# Mollycoddle Command Line Options

The command line options for Molly are as follows:

```text
❯ mollycoddle <PATH> -rulesfile=<PATH> -primaryRoot=<PATH> -formatter=<plain|azdo> -warnonly -addrulehelp  -disabled -debug=<DEBUG>  -version=<VERSION> -temppath=<PATH> -fix
```

### -dir  (or default single parameter) (required)

The directory to scan, mollycoddle should be run from the parent directory e.g. mollycoddle .\codetoscan  that will give the most reliable results.

### -rulesfile (required)

The location of a rules file - either a mollyset collection of rules to execute or a single .molly rule to execute.  Typically rules are grouped into single mollyset rules files.   Where XXVERSIONNAMEXX is placed in the path of this location then the -version parameter must also be used.  The -version parameter will replace XXVERSIONNAMEXX with the value supplied.  This enables you to use different versions of rules ( e.g. -version=latest).  

### -primaryRoot

The root of the location for the primary copies of files. When using single primary copy files such as nuget.config or .editorconfig this location specifies where the primary copies are sourced from. Can be a directory path, a UNC path or a Nexus repository path. See path formatting below.

### -formatter

This determines the format output for messages written to the console.  Options are:

plain - Writes standard format text.
azdo - Writes text formatted for azure devops pipelines

```text
Azdo Output:
##vso[task.logissue type=error]Error: Error - Unable To Read RulesFiles

Plain Output:
Error: Error - Unable To Read RulesFiles
```

### -warnonly

Performs the full mollycoddle checks but returns zero upon completion. 

### -addrulehelp

Includes additional help links in the output to support simpler resolution of issues found in the structure.

### -disabled

Disables all checks and returns zero to the calling process, most often used to temporarily disable mollycoddle in build pipelines.

### -debug=\<debug control string>

Allows the specification of a debug string to write additional in depth debugging information.  Use v-** for verbose. Other values should only really be required when contacting support.

### -version=\<version>

Allows you to specify a value that will be used to replace the XXVERSIONNAMEXX string in the rules file location.  This allows you to load different versions of rulesets using the command line to target the correct ruleset.  This is designed for use cases where standards change and therefore you can fix a set of standards at a specific point.  The text passed in is just a text identifier so can be anything that works in a path name.

```batch
// This will look for the rules in c:\files\default\defaultrules.mollyset
mollycoddle dir -rulesfile="C:\Files\XXVERSIONNAMEXX\defaultrules.mollyset" -primaryRoot="C:\Files\PrimaryFiles" -formatter="plain" -version=default
```

### -temppath

Specifies the working directory for cached files to be placed.  Defaults to system temporary path.

### -fix

New feature: Applies a fix for rule violations. Currently only fixes for FileStructure/MustMatchWithPrimary rules are implemented.