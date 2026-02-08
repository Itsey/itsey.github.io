Issues / FAQ
==============================================

## No Trace?

The most common problem is that trace is not being written.  Here are some examples of why that might be the case:

### No Trace - Process Exit.

If your process exits very quickly then there is not enough time to write the trace to the output stream.  Take this code:

```csharp
program main() {
     Bilge bx = new Bilge("test");
     bx.ActiveTraceLevel = SourceLevels.Verbose;
     b.AddHandler(new TCPHandler("127.0.0.1",9060);
     b.Info.Log("Hello World");
  }
```

This program will execute, but depending on the speed of the hanlder used there may not be any trace written.  Bilge uses a background thread for all of its trace processing to minimise the time that it is disrupting execution of your program and therefore when the main process exits so does the background thread.   

To resolve this add `b.FlushAndShutdown()` to the end of the code.  This is not typically necessary but in examples such as this (and may starter examples) it helps ensure trace is written.  If your code is also experiencing crashes the a `b.Flush()` can help ensure trace is written prior to program termination.    Note that if you are using the network listener to write to Flim Flam then even with the flush it is likely that the network transmission will not complete before program exit. Therefore its often required to put a small sleep in the program at the end for debugging purposes if you wish to ensure that network trace is written completely before program exit.

### No Trace - Default Configuration.

If you have not set a trace level on any Bilge instance then no trace will be written as by default trace is off for each instance of bilge.  This example::

    program main() {
        Bilge b = new Bilge("Default");
        b.AddHandler(new TCPHandler("127.0.0.1",9060);
        b.Info.Log("Hello World");
    }

Will not generate any trace output, as the default trace level for Bilge is TraceLevel.Off.  To correct this either pass the trace level on the constructor or set the trace level in the code

### Using DiagnosticStatus.

Its possible to query bilge for status to see if that can give you a clue as to what is wrong.

```csharp
Console.WriteLine(b.GetDiagnosticStatus());
```

This will output something like the following:

```text
__ Logging __
Trace Level Verbose
 Info Writing: True
 Verbose Writing: True
 Warning Writing: True
 Error Writing: True
__ Router __
 Router Total Handler Errors: 0
 Router Suppress Errors: False
__ Handlers __
QueueDepth: 0 Max: -1
___________________________
Handler RollingFileSystemHandler
Last Exception: Could not find a part of the path 'C:\Temp\_Deleteme\Lg\log_2772022.log'. total exceptions 1
```

This gives you a status on the loggin state of Bilge, along with some information on routing and finally a view on each of the handlers that has been loaded.  In this instance one of the handlers failed with the error message there.
