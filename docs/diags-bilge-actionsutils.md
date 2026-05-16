### Actions and Utilities

| Bilge Reference | Quick Links    |
|-----------------|----------------|
| [Basic Logging](diags-bilge-basiclogging.md)  | [Tips: AutoTracing Unit Tests](diags-bilge-tips-autoxunit.md) |
| [Additional Options](diags-bilge-options.md)  | [Actions and Utils](diags-bilge-actionsutils.md)|
| [Configuration Resolvers](diags-bilge-configurationResolvers.md) | |
| [Handlers](diags-handlers-index.md)| |


## Using Actions

Actions are a quick way of gaining access to parts of the code base and taking an action using the tracing framework.  This can be used for additional logging or for unit tests or other diagnostic approaches.

### Quick Example

Consider a retry loop around a file to check for sharing violations, to test this somehow the file must be locked the first time round the loop then freed at some point in the loop.  One way of doing this is with a callback inside that method and bilge Actions provide this approach.


```csharp
Bilge b = new Bilge("some-file-accessor");

SomeFileUpdateMethod() {
...
 } catch (IOException ex) when (ex.HResult == SHARINGVIOLATIONHRESULT) {
     b.Action.Occured("retry", $"count {count}");
     if (count >= MAXRETRIES) {
         b.Error.Log($"Error, file still locked.");
         throw;
     }
     Task.Delay(RETRYDELAYMS).Wait();
 }
```

Then in the corresponding unit test.

```csharp
  FileStream? fl = null;
  Action<IBilgeActionEvent> act = (a) => {
      fl?.Close();
  };
  
  nsb.Action.RegisterHandler(act, HANDLERNAME);
  
 // lock the file
 fl = File.Open(targetFilename, FileMode.OpenOrCreate, FileAccess.Write, FileShare.Read);
 
 // call the method with the retry loop 
 SomeFileUpdateMethod()
```


## Registering Actions


```csharp
 var sut = TestHelper.GetBilgeAndClearDown();
 _ = sut.Action.RegisterHandler((x) => {
     Assert.Equal(1, x.CallCount);
 }, "default");

 sut.Action.Occured("test", "dummy");
```

## Unregistering Actions

```csharp
 System.Action<IBilgeActionEvent> action = (x) => {
     actionCount++;
 };
 bool res = sut.Action.RegisterHandler(action, "default");
 sut.Action.UnregisterHandler(action, "default");
```