## Performance Logging


| Bilge Reference | Quick Links    |
|-----------------|----------------|
| [Basic Logging](diags-bilge-basiclogging.md)  | [Tips: AutoTracing Unit Tests](diags-bilge-tips-autoxunit.md) |
| [Additional Options](diags-bilge-options.md)  | |
| [Configuration Resolvers](diags-bilge-configurationResolvers.md) | |
| [Handlers](bilge-handlers-index.md)| |

### 1 - Run it without the debugger.

Quite often when testing you will notice slowdowns in the code during your normal debugging cycle.  Its important that you measure the slowdowns when running outside of the debugger, especially when using things like tracepoints.  Prior to optimising the code capture the timings without using the debugger.  

### 2 - Turn It OFF!

Needless to say Tracing adds overhead, to eliminate the overhead turn off the tracing.  Consider whether you need tracing on at all or whether you can set it to error mode.  If you do need tracing do you need to log it all to the disk or only log to the disk on error?

### 3 - Use Callbacks.
Typically when code is executed the runtime evaluates the parameters to pass into the method, in some cases creating the log string itself can be quite time consuming.  While you require the logging this is a worthwhile trade but if logging is off or set to a level more verbose than is currently active this can cause an unnecessary delay.

To avoid this problem use the log callback overloads - consider the code below, the string that is to be output is artificially slow to create.  The logging using the string is in Verbose mode yet by default the error mode instance of Bilge will incur the performance hit as the parameter is passed ( therefore the method called ) prior to the logic to check if the logigng is writing.

To avoid this use the callback:


```csharp
 private static string SomeSlowString() {
     Thread.Sleep(10);
     return new Random().Next(0, 100).ToString();
 }
        
Bilge berror = new Bilge("context", tl:SourceLevels.Error);
Bilge bverbose = new Bilge("context", tl:SourceLevels.Verbose);

// Directly creating the string  
berror.Verbose.Log(SomeSlowString());
bverbose.Verbose.Log(SomeSlowString());
            
// Using a callback to create the string
berror.Verbose.Log(() => { return SomeSlowString(); };
bverbose.Verbose.Log(() => { return SomeSlowString(); };


```

The output on my machine:


```text
errorbilge-nocallback > 1578ms @ Error
verbosebilge-callback > 1591ms @ Verbose
errorbilge-nocallback > 1591ms @ Error
errorbilge-callback > 0ms @ Error
```

Notice that the Verbose logging bilge has no choice but to incur the hit - that's how the logs are written, but the error logging bilge that uses the callback incurs minimal overhead when it is not being logged.

Note also that this only makes a difference where building the trace string incurs additional cost - such as with the artificially slow method above - removing the Sleep from the code above means all approaches return in 0 or 1 ms.


### Write On Fail.


