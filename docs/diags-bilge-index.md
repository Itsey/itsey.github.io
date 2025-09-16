### Bilge

Bilge itself is the main trace library.  The documentation here covers using the various features and functions of bilge.

| Bilge Reference | Quick Links    |
|-----------------|----------------|
| [Basic Logging](diags-bilge-basiclogging.md)  | [Tips: AutoTracing Unit Tests](diags-bilge-tips-autoxunit.md) |
| [Additional Options](diags-bilge-options.md)  | |
| [Configuration Resolvers](diags-bilge-configurationResolvers.md) | |
| [Handlers](diags-handlers-index.md)| |

#### Basic Logging

Basic logging is what you will use most of the time.  You can write out trace statements, timings and program flow using a variety of logging commands at different trace levels.  See the following documentation:
[Basic Logging](diags-bilge-basiclogging.md)

#### Logging Principals

The library is based around the idea that you should debug your code from your trace as early as possible.  Doing this ensures you have the right level of trace in the code so that when you do find 
that you need to debug your code from just your trace statements then you have a chance at being able to do that.  To this end the library places a lot of emphasis on how we send trace statements and also how they are made visible to the programmer.

To this end the primary use case for Bilge is with a tool called FlimFlam which is a dynamic trace viewer. During production the text files can be used to capture the logging but during development FlimFlam
is used to visualise logs in real time.  This real time visualisation is how we get an understanding of how effective the logs are prior to finding issues in production.

#### Performance Considerations.

Tracing can take quite a bit of overhead and Bilge, by default, adds a lot of verbosity to the trace output, knowing how to configure Bilge and your trace statements efficiently can make a lot of difference to the performance of your trace code.
[Performant Logging](diags-bilge-performance.md)

### Transient Data

Occasionally you need data that transitions very quickly - there is a special implementation of transient data in Bilge which can be represented in flimflam.  It is not a common scenario - see [Using Transient Data](diags-bilge-transientLogging.md)

#### Actions
Actions are a specific use case - there are times when you need to know whether the code has done something, either for the point of view of testing or to be sure features are working as expected.  
DocumentsNotImplementedException();

#### ILogger Support

There is a feature of ToILogger which is not recommended. This implements basic ILogger support for Bilge where Ilogger is needed to be used.  However this negates much of the benefit of the bilge
interface and therefore is not recommended for general use.  To use ILogger effectively either use your own ILogger support or use a different library that is designed to work with it.  The ToILogger 
function is purely designed for where no alternative is available ( e.g. calling into another library of code that requires an ILogger interface).

#### Utilities

DocumentsNotImplementedException();