### Bilge Options

Bilge itself is the main trace library.  The documentation here covers using the various features and functions of bilge.

| Bilge Reference | Quick Links    |
|-----------------|----------------|
| [Basic Logging](diags-bilge-basiclogging.md)  | [Tips: AutoTracing Unit Tests](diags-bilge-tip-autoxunit.md) |


#### Adding Information To the Trace Stream

Using the ConfigureTrace method additional configuration can be applied.

##### Adding Class Names

You can include class name information in the trace messages, sometimes this can help providing a more detailed view of where trace is occurring and it allows for additional filtering (e.g. exclude all by class).

To enable class information:

```csharp
Bilge b = new Bilge();
b.ConfigureTrace(new TraceConfiguraton() {
    AddClassDetailToTrace = true
});
```

Warning - this creates a stack trace on each log, and therefore will incur a non trivial performance overhead, this should not be enabled when performance is critical.


