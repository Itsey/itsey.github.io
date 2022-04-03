
Plisky.Diagnostics is available [on Nuget](https://www.nuget.org/packages/Plisky.Diagnostics/).

1. Right click on your solution and choose Manage Nuget Packages.
2. In Browse, type Plisky. and wait for the package selection to display.
3. Select Plisky.Diagnostics and tick all projects in your solution that are .net and choose Install
4. Select Plisky.Listeners and tick your startup project and choose Install

Now you have Plisky.Diagnostics installed Bilge will be available for use in your code.

At your Application Startup location enter the following code:

***

```
Bilge b = new Bilge();
b.AddHandler(new SimpleTraceFileHandler());
b.Info.Log("Hello Cruel World");
```

When the application is run and exits a file called bilgedefault.log will be created in your %TEMP% folder.  This handler is designed to get you up and running very quickly but its not very usefull.  You will more usually be using other handlers that do different things.  For details see the [handlers pages](https://github.com/Itsey/Plisky.Diagnostics/wiki/Diagnostic-Handlers).


The main value from Blige occurs when combined with FlimFlam - the viewer. Legacy FlimFlam is available as a release 1.0 and offers the view of the trace output issued by Bilge.  Get the binary here: https://github.com/Itsey/Plisky.FlimFlam/releases/tag/LegacyFF10

The most common usage on Development Machines is using the TCPHandler to write locally to the machine running FlimFlam (or across the network if that's appropriate for your environment).  The default port for FlimFlam to listen on is 9060.

```
Bilge b = new Bilge();

// You must enable tracing
b.SetTraceLevel = SourceLevel.Verbose;

// To get any output you must use a handler - this is one talking to FlimFlam
b.AddHandler(new TCPHandler("127.0.0.1",9060));

// Your Logging Goes Here.
b.Info.Log("Hello Cruel World");

// Not normally required but for small test apps that close quickly it can help
b.FLush();
```

#### A Note On Security

The listener package contains the TCPListener - which communicates across the network to an address you set.  Do not be surprised if your firewall or security software detects this as an outbound connection.  

Similarly if you use FlimFlam it will attempt to listen on port 9060 - and it is designed for use within a protected network (i.e. FlimFlam is not designed to be internet facing).

If you do not want any connections then disable listening to network connections in FlimFlam and either do not use the TCPListener class or do not use Plisky.Diagnostics.Listeners at all, but instead provide your own listener implementation.

