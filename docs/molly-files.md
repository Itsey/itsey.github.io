### Mollycoddle Rules Files

There are two types of files

* <name>.mollyset - This is a set of rules files, one per line each referencing a .mollyfile
* <name>.molly - This is a single or multiple rule implementation file.


#### Molly Set Files

Mollysets are simply text files with the name of one rules file on each line:

```text
#This is a comment line
alledconfig.molly
allgitignore.molly
allnugetconfig.molly
```

A molly set file will look for each of the named rules files in the same directory and load it into mollycoddle.  This is a fine grained way of grouping up individual molly rules into collections.  Lines that start with a # character are ignored and serve as comments.


### Molly Rules Files

Molly rules files are collections of rules implementations themselves.  A molly file can contain one or more rules, and each rule follows the same format:

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

There is more detail on how to create these files here [Creating Molly Rules](createrules.md)