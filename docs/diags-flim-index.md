| Bilge Reference | Quick Links    |
|-----------------|----------------|
| [Basic Logging](diags-bilge-basiclogging.md)  | [Tips: AutoTracing Unit Tests](diags-bilge-tips-autoxunit.md) |
| [Additional Options](diags-bilge-options.md)  | [Actions and Utils](diags-bilge-actionsutils.md)|
| [Configuration Resolvers](diags-bilge-configurationResolvers.md) | |
| [Handlers](diags-handlers-index.md)| |



### Flim Flam

Flim Flam is a supporting GUI app for log file analysis. You can simply log data to a text file and use your own tools to inspect the logs that Bilge creates, but wheres the fun in that?



## Developer Real Time Trace



The main reason to use trace is to help debugging problems, usually in production.  If you have not spent time debugging from your logs in development then it is exceedingly unlikely that you will find them useful in production.   To solve this problem you should be looking at the logs constantly when building code.  However this involves opening up the log file, usually after the app has run.  This is an ineffective way of working.



FlimFlam is a UI that uses network streams to display the logs in real time.  



You should ALWAYS use the latest FlimFlam formatter when working with FlimFlam - currently FlimFlamV4Formatter.



#### Contents

* [Analysing Timings](diags-flim-tracetimings.md)