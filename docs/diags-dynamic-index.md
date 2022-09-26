### Dynamic Trace Configuration

Dynamic trace configuration is used when you want to configure trace while the application is running.  Quite often trace is enabled prior to the code running and lives at a set level for the life of the code.  However this is sometimes restrictve and its sometimes desirable to configure trace on the fly.

#### Overview

The configuration resolver, is a common way to configure Bilge instances.  However this happens at the time that the instance is created, therefore once you set a configuration resolver it will only be picked up by new instances of bilge.

```csharp
var b = new Bilge("instance");
Bilge.SetConfigurationResolver("v-**");
var b2 = new Bilge("instance2");

b.Info.Log("X");   // Will not log, the instance was created before the resolver was set.
b2.Info.Log("XX"); // Will log, created after the configuration resolver was set.
```


Unless you use the default instance throughout your code base you will also find that you need to keep tabs on the instances of Bilge if you want to set the trace level consistently.  

#### Using DynamicTrace

You can use the built in DynamicTrace support to help with some of these issues.

```csharp
var sut = new DynamicTrace();
var b = sut.CreateBilge("test");
```

The DynamicTrace CreateBilge method will create an instance of Bilge and keep a handle to that instance so that future configurations can be applied.  For example

```csharp
var sut = new DynamicTrace();
var b1 = sut.CreateBilge("test");
var b2 = sut.CreateBilge("test2");

sut.SetTraceLevel(SourceLevels.Verbose);
// both b1 and b2 are now set to Verbose.
```


#### Using Session Filters

Session filters are designed to throw away trace information that is not valuable, for example if you want to trace a specific session or user flow through a system.  These filters use the existing context information but they will throw away trace where the context does not match. Be careful as you can loose a lot of trace data this way.

```csharp
   sut.SessionContext = "userid-1234";
   sut.SetSessionFilter("userid-1234");

   sut.Verbose.Log("Hello World"); // Will be written
   
   sut.SetSessionFilter("bob");
   sut.Verbose.Log("Hello World"); // Will be Lost
```

Session filters can work with any of the session data that is available to the message, you can specify your own function to work with this.

```csharp
  sut.SetSessionFilter( contexts => {
     if (contexts["KEY"] == "Value")  {
          return true; // Keep this message
     }
     return false; // Throw away all other messages.
  });
  
  // Bilge.BILGE_SESSION_CONTEXT_STR constant holds KEY for session context.
```





