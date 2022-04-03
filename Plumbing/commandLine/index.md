# Command Line Parser.


The CommandLine Parser is there to support working with command lines using Plisky.Plumbing to do the argument parsing and manipulating the data into a class that
you provide. The class uses attributes to map the command line arguments to the class members.



### Quick Start.


To start first define your arguments class:


```csharp
[CommandLineArguments]
public class SampleCommandLine_C2 {
  [CommandLineArg("Filename", Description = "The filename to be ~MatchShortDescrFilename~~ passed into the application", IsSingleParameterDefault  true)]
  public string Filename;

  [CommandLineArg("INTVALUE")]
  public int NumberParam1 { get; set; }

  [CommandLineArg("L")]
  [CommandLineArg("LONGVALUE")]
  public long NumberParam2 { get; set; }
}
```


Command line arguments are decorated with an attirburte that specifies the name of the command line argument.  Multiple attributes can be specified to allow for
short and long named arguments.  

Once this is done use the CommandLineArgumentSupport class to parse the arguments.

```csharp
var clas = new CommandArgumentSupport {
                ArgumentPostfix = ":",
                ArgumentPrefix = "",
                ArgumentPrefixOptional = true
            };
var argsClass = new SampleCommandLine_C2();
clas.ProcessArguments(argsClass, args);
```

The postfix and prefix determines how the arguments are passed - in this instance arugments should be passed like this

'Filename:"This is the filename.txt" INTVALUE:1234 L:5678

There is no specified prefix on the arguments, but a postfix of : is used.    Long strings and special characters (like | and <>) can be provided by putting quotes around
the argument.

### Alternative Generic Syntax.

There is an alternative generic based syntax that will save you a tiny bit of typing at the cost of a tiny bit of runtime overhead.  If you prefer the syntax the 
functionality is identical.  This will only work for argument attributed classes that have a default constructor.

```csharp
var argsClass = ProcessArguments<SampleCommandLine_C2>(args)
// Is the same as
var argsClass = new SampleCommandLine_C2();
clas.ProcessArguments(argsClass, args);
```


### TroubleShooting.


To enable trace for ConfigHub turn on the trace switch "Plisky-CLAS".
This can be done using the standard configuration for Plisky.Diagnostics (see here)