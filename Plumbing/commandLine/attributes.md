#  Command Line Parser - Attributes.


All of the argument parsing elements for the command line parser are done via attributes on the class that you want the data to be placed into.  These attributes control how
the command line is parsed. For all of the examples below it is assumed there is a prefix of - and a postfix of =

### All Attributes


```csharp
[CommandLineArg("first")]
pulic string First { get; set; }
```

Assigns a value that the named argument First recieves on the command line.  e.g.  tool.exe -first=value

```csharp
[CommandLineArg("first")]
[CommandLineArg("f")]
pulic string First { get; set; }
```
Allows multiple names to match to the same value.  e.g. tool.exe -f=value is the same as tool.exe -first=value

```csharp
[CommandLineArg("M",IsRequired =true)]
public string Mandatory { get; set; }
```

IsRequired will throw an error if the argument is not present when the parsing is completed.  Used for mandatory arguments.

```csharp
[CommandLineArg("X1", Description = "The name of an XSLT file to use in the transform.")]
public string TransformFilename1;
```
Description will set the description of the argument, used for printing help.  

```csharp
[CommandLineArgDefault]
public string[] Remainder { get; set; }
```

A default is one or more strings that do not have names specified.  Used like this the remaining arguments that were not mapped
will be placed into this array

```csharp
[CommandLineArg("StrArray",ArraySeparatorChar =";")]
public string[] StrArray { get; set; }
```
Array separator char will be used to turn a list of values into an array - e.g. tool.exe -StrArray=one;two;three;four, will populate the array with
[ "one", "two", "three", "four" ].


