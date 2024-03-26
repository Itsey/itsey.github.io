### Mollycoddle

> For when you just cant let the babbers code on their own.

MollyCoddle is a directory and file linting solution for source control projects designed to check the structure of the source repository rather than the code itself.  It is NOT a code linting solution there are plenty of those out there already.


#### Mollycoddle Quick Start Guide.

#### Getting started with Mollycoddle

Mollycoddle needs three things to run:
* The mollycoddle executable.
* A source code structure to execute against.
* A set of rules to execute.



#### Using Mollycoddle from the command line.

Execute MollyCoddle from the command line, passing a parameter of the path to scan and a -masterRoot parameter for using master files.

```text
mollycoddle.exe <RootOfRepository> [-masterRoot=<PathToMasterFileS>] [-rulesfile="<PathToMollyRulesFile"] [-Disabled]
```

RootOfRepository - this should be the root where the repository is located to scan.  
PathToMasterFiles - If you are using rules that compare files against their master files then this should be the path to where the master. versions of the files live.
RulesFile - This is the set of rules that you want Mollycoddle to execute
Disabled - If this is specified MollyCoddle does nothing and returns success
Output - Specify output type - Default writes to std out, AZDO  writes pipeline formatted strings to std out

For the full command line options see this link - [MollyCoddle Command Line](molly-commandline.md)

```text
❯ .\mollycoddle.exe C:\Files\Code\git\mollycoddle -masterRoot=C:\Files\Code\git\mollycoddle\src\_Dependencies\TestMasterPath\
```

When mollycoddle executes violations will be returned to standard output.  The number of violations will be returned as the exit code for using in automations and scripts.

```text
❯ .\mollycoddle.exe C:\Files\Code\git\mollycoddle -masterRoot=C:\Files\Code\git\mollycoddle\src\_Dependencies\TestMasterPath\
Violation M-0004 (c:\files\code\git\mollycoddle\.gitignore does not match master.)
Total Violations 1
```

### Using Mollycoddle in Nuke.

Nuke is a build tool for .net - to use Mollycoddle in nuke see [this guide.](molly-nuke.md)

### MollyCoddle Rules

You can create your own rules but the default set are referenced here [MollyCoddle Rules](molly-rules.md)


### Creating Your Own Rules

Mollycoddle reads rules from different file types - to find out about sets and molly rules the documentation is here [MollyCoddle Rules Files](molly-files.md)
