[Home](index.md) | [Mollycoddle](molly-index.md) | [Command Line](molly-commandline.md) | [QuickStart](molly-quickstart.md) | [CreateRules](molly-createRules.md) |  [Nuke](molly-nuke.md)

# Mollycoddle

> For when you just cant let the babbers code on their own.

MollyCoddle is a directory and file linting solution for source control projects designed to check the structure of the source repository rather than the code itself.  It is NOT a code linting solution there are plenty of those out there already.


### Mollycoddle Quick Start Guide.

#### Getting started with Mollycoddle

Mollycoddle needs three things to run:

* The mollycoddle executable.
* A source code structure to execute against.
* A set of rules to execute.

Mollycoddle is available as a dotnet tool, to install run `dotnet tool install Plisky.Mollycoddle`.

You should first take a look at the rules [MollyCoddle Rules](molly-rules.md) to see what sorts of things Molly can do.  Once you have seen the rules you'll need to determine where you will store your rulesfiles.  This can be on a local path for local use, or on a network share for a local team or a nexus repository for centralisation.   When Molly loads it will attempt to retrieve the rules files and any other data from the location you specify. A [QuickStart](molly-quickstart.md) guide is available.

#### Using Mollycoddle from the command line.

Execute MollyCoddle from the command line, passing a parameter of the path to scan and a -primaryRoot parameter for using common
files.

```text
❯ .\mollycoddle.exe <RootOfRepository> [-primaryRoot=<PathToCommonFileS>] [-rulesfile="<PathToMollyRulesFile"] [-Disabled]
```

RootOfRepository - this should be the root where the repository is located to scan.  
PathToPrimaryFiles - If you are using rules that compare files against their primary files then this should be the path to where the common versions of the files live.
RulesFile - This is the set of rules that you want Mollycoddle to execute
Disabled - If this is specified MollyCoddle does nothing and returns success
Output - Specify output type - Default writes to std out, AZDO  writes pipeline formatted strings to std out

For the full command line options see this link - [MollyCoddle Command Line](molly-commandline.md)

```text
❯ .\mollycoddle.exe C:\Files\Code\git\mollycoddle -primaryRoot=C:\Files\Code\git\mollycoddle\src\_Dependencies\TestMasterPath\
```

When mollycoddle executes violations will be returned to standard output.  The number of violations will be returned as the exit code for using in automations and scripts.

```text
❯ .\mollycoddle.exe C:\Files\Code\git\mollycoddle -primaryRoot=C:\Files\Code\git\mollycoddle\src\_Dependencies\TestMasterPath\
Violation M-0004 (c:\files\code\git\mollycoddle\.gitignore does not match primary.)
Total Violations 1
```

### Using Mollycoddle in Nuke.

Nuke is a build tool for .net - to use Mollycoddle in nuke see [this guide.](molly-nuke.md)

### MollyCoddle Rules

You can create your own rules but the default set are referenced here [MollyCoddle Rules](molly-rules.md).

#### Creating Your Own Rules

Mollycoddle reads rules from different file types - to find out about sets and molly rules the documentation is here [MollyCoddle Rules Files](molly-files.md).
