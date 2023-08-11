### Building A Custom Handler

Custom handlers can be used to put the trace format out in any format that you wish, this allows for integration with other logging tools or writing in a format suited for your needs.

#### Creating a custom handler

This is the simplest form of custom handler, although it does not actually do anything it is a valid handler.

```csharp
 public class CustomHandler : BaseHandler, IBilgeMessageListener {

        public string Name => "CustomHandler";

        public string GetStatus() {
            return "OK";
        }

        public Task HandleMessageAsync(MessageMetadata[] msg) {
            return Task.CompletedTask;
        }
    }
```


We are going to want to do something more useful though - lets put the message out to the console in JSON format, using the system JsonSerializer we can write JSON to the console.


```csharp
 public class CustomHandler : BaseHandler, IBilgeMessageListener {

        public string Name => nameof(CustomHandler);

        public string GetStatus() {
            return "OK";
        }

        public Task HandleMessageAsync(MessageMetadata[] msg) {

            foreach (var nextMsg in msg) {
                var f = JsonSerializer.Serialize(nextMsg, typeof(MessageMetadata));
                Console.WriteLine(f);
            }
            
            return Task.CompletedTask;
        }
    }
```
![Json on the console](assets\images\bilge-handler-jsonconsole.png)


Note that the HandleMessageAsync is an async method, returning a task therefore if you are writing to a network or filesystem you can use the async methods to not block.

The main elements of the MessageMetadata structure that are passed in are:

```csharp

// Contains the type of command - log message, verbose message, flow message etc.
TraceCommandTypes CommandType;  

// Contains the main message that has been logged.
string Body;

// Contains secondary information passed to the log
string FurtherDetails;

// Contains supporting context data information
IDictionary<string, string> MessageTags;

// Contains structured logging information.
dynamic StructuredData { get; set; }

```

### Using Message Status

The handlers have a status method, this is used to indicate issues with the handler for diagnostic purposes - when no trace is being written its a good idea to be able to find out why that is. You should return a status of your handler wherever possible.

```csharp
 public class CustomHandler : BaseHandler, IBilgeMessageListener {
        private string statusString = "OK";
        public string Name => nameof(CustomHandler);

        public string GetStatus() {
            return "OK";
        }

        public Task HandleMessageAsync(MessageMetadata[] msg) {
            try {
                foreach (var nextMsg in msg) {
                    var f = JsonSerializer.Serialize(nextMsg, typeof(MessageMetadata));
                    Console.WriteLine(f);
                }
            } catch (NotSupportedException nx) {
                statusString = "HandlerFailed > " + DateTime.Now.ToString() + " > " + nx.Message;
            }
            return Task.CompletedTask;
        }
    }
```

