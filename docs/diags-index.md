# Plisky.Diagnostics - Documentation.


[Plisky.Diagnostics](https://github.com/Itsey/Plisky.Diagnostics/wiki) is the assembly hosting Bilge, a .net trace library dating back to the first days of .net being available.    

Bilge uses methods to write trace information and handlers to disperse this information.  While it can be used to write to the filesystem or logs its most commonly used to write via the network to the dedicated viewer -  [FlimFlam](https://github.com/Itsey/Plisky.FlimFlam/wiki). FlimFlam is used to display and interrogate trace logs and is designed to work with Bilge.

#### Guidance Documents

* [Quick Start](diags-guide-quickstart.md)
* [Release Notes](diags-releases.md)
* [Flim Flam](diags-flim-index.md)
* [Bilge Reference](diags-bilge-index.md)
* [Listeners](diags-handlers-index.md)
* [Troubleshooting](diags-guide-troubleshooting.md)
* [Performance Analysis](diags-guide-performance.md)


#### Developer Focused Trace

Bilge and [FlimFlam](https://github.com/Itsey/Plisky.FlimFlam/wiki) are designed to be developer focused trace - providing real time trace information from your application to aid in debugging production problems.  

The goals of the implementation are:

* Default To Least Impact In Production
* Empower the developer to resolve complex issues
* Expect to be the cross cutting trace library when used


These goals shape the way that Bilge is delivered - having most settings off by default you will find you need to configure trace before it is written.  This is a developer centric trace solution and is therefore not the same as application logging.   

Typically applications require both a logging solution and a trace solution but the purposes and approaches are very different, most notably trace expects to be off in production unless something goes wrong or you are looking out for a problem.  Application logging on the other hand is often on in production and is more aimed at calling out issues and metrics rather than identifying root cause of problems.

Due to the different use cases the priority for Bilge is to empower developers to get to root cause rather than say performance or low impact when turned on.


