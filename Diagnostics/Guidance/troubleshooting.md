
Issues / FAQ
==============================================


## No Trace?

The most common problem is that trace is not being written.  Here are some examples of why that might be the case:


### No Trace - Process Exit.

If your process exits very quickly then there is not enough time to write the trace to the output stream.  Take this code:

```csharp
program main() {
        Bilge b = new Bilge(tl:TraceLevel.Verbose);
        b.AddHandler(new TCPHandler("127.0.0.1",9060);
        b.Info.Log("Hello World");
    }
```

This program will execute, but it's exceedingly unlikely that any trace will be written.  Bilge uses a background thread for all of its trace processing to minimise the time that it is disrupting execution of your program. 

As a result if a process exits before Bilge has had a timeslice then trace will not be written, additionally as the thread is registered as a background thread it will not prevent the ending of the process.

To resolve this add `b.Flush()` to the end of the code.  This is not typically necessary but in examples such as this (and may starter examples) it helps ensure trace is written.  If your code is also experiencing crashes the a flush can help ensure trace is written prior to program termination.


### No Trace - Default Configuration.

If you have not set a trace level on any Bilge instance then no trace will be written as by default trace is off for each instance of bilge.  This example::

    program main() {
        Bilge b = new Bilge("Default");
        b.AddHandler(new TCPHandler("127.0.0.1",9060);
        b.Info.Log("Hello World");
    }

Will not generate any trace output, as the default trace level for Bilge is TraceLevel.Off.  To correct this either pass the trace level on the constructor
or set the trace level in the code.





