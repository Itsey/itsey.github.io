### Configuration Resolvers


The configuration resolver exists to enable trace to be turned on dynamically by your application configuration.   There are many ways of solving this particular problem and if you use Dependency injection then some are more elegant.   This approach is aimed at the core principal of making it simple for the developer.

Previous versions of Bilge had dependencies on trace sources and switches and so on.  As the logging approach of the framework seems to change rather more often than the Bilge interface this was all removed from Bilge and instead the custom configuration resolver approach was taken.

### The simplest configuration resolver

The simplest resolver simply turns on trace for everything, all the time.

``` csharp
Bilge.SetConfigurationResolver((instanceName, lvl) => {
    return SourceLevels.Verbose;
}
```

No matter what the configuration of the Bilge instances that are created are they will all have their initial trace state set to verbose.  Note that when you use libraries that also use Bilge this will enable their verbose logging too.   It is therefore more common to use a string to identify which trace areas to turn on.

``` csharp
Bilge.SetConfigurationResolver((instanceName, lvl) => {
    if (instanceName.Containes("myproduct")) {
        return SourceLevels.Verbose;
    } else { 
        return lvl;
    }
}
```
This is a simple and common solution - turn on trace only where the instances of bilge are using the name "myproduct".  This is paired with code in your solution to initialize Bilge with an instance name:

``` csharp
Bilge b = new Bilge("myproduct-repository");
```

### Default String Configuration Resolver

Using a string to manage a configuration resolver is such a standard task that there is an inbuilt method for supporting this.

```csharp
        // public static Func<string, SourceLevels, SourceLevels> SetConfigurationResolver(string crInitialisationString)
        Bilge.SetConfigurationResolver("e-**");
```

This uses a string notation to set a configuration resolver to a level.  Each option is ; delimited. 
e- set to error    
v- set to verbose    
w- set to warning    
** match all items    
text* match anything starting with text    
*text match anything ending with text    

Examples:

```text
1  e-*bob;v-mik*    
2  none
3  e-**
4  v-**
```
1 Anything ending in Bob is set to trace at error ( MyClassBob ) anything starting with Mik is set to verbose. ( MikClass ).  MikclassBob is set to error as its in order.    
2 turn all trace off    
3 turn all trace to Error    
4 turn all trace to verbose    


### Complex Configuration Resolvers

Configuration resolvers aim to pass the "problem" of whether to enable trace back to the developer.  This is an example of a more complex resolver that uses a fallback system to try and enable based on trace source or trace switch names in application config.

```csharp
Bilge.SetConfigurationResolver((instanceName, lvl) => {
    // Logic -> Try Source Switch, Failing that Trace Switch, failing that SourceSwitch + Switch, Failing that TraceSwitch+Switch.

             SourceLevels result = lvl;
             bool tryTraceSwitches = true;

             try {
                 SourceSwitch ss = new SourceSwitch(instanceName);
                 if (ss.Level == SourceLevels.Off) {
                     ss = new SourceSwitch($"{instanceName}Switch");
                     if (ss.Level == SourceLevels.Off) {
                     } else {
                         tryTraceSwitches = false;
                         result = ss.Level;
                     }
                 } else {
                     tryTraceSwitches = false;
                     result = ss.Level;
                 }
             } catch (SystemException) {
                 // This is the higher level exception of a ConfigurationErrorsException but that one requires a separate reference
                 // This occurs when a TraceSwitch has the same name as the source switch with a value that is not supported by source switch e.g. Info
             }

             if (tryTraceSwitches) {
                 TraceSwitch ts = new TraceSwitch(instanceName, "");
                 if (ts.Level == TraceLevel.Off) {
                     ts = new TraceSwitch($"{instanceName}Switch", "");
                     if (ts.Level != TraceLevel.Off) {
                         result = Bilge.ConvertTraceLevel(ts.Level);
                     }
                 } else {
                     result = Bilge.ConvertTraceLevel(ts.Level);
                 }
             }

             return result;
         });
```
