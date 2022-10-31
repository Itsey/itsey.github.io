### Tracing Program Flow

Tracing program flow is one of the most common activities when trying to diagnose faults in your code, mostly this is what tracing is used for to try and follow the flow of execution that a user takes through your source code to work out where the issue lies.


Tracing program flow with Bilge has multiple different facets and these can get quite complex.

### Creating Bilge Instances

Before you can use Bilge logging you should create an instance name.  This instance name provides additional context for that instance of bilge, typically this is used to identify sub systems of the application.  For example naming the context appname-subsystemname means that its possible to enable trace on specific subsystems.   

Of course how you name your instances for context is entirely up to you


```csharp
Bilge b = new Bilge();                           // Default no instance name
Bilge b2 = new Bilge("app-main-instance");       // Context provided in instance name
Bilge b3 = new Bilge("app-repo-database");       // Identify sub system - repository - sub sub system - database
Bilge b4 = new Bilge($"customer-{customerId}");  // Trace per customer rather than by subsystem
```

Typically you will create an instance of Bilge anywhere that you need trace - most commonly in classes:

```csharp
public class Test {
    // Create instance
    protected Bilge b = new Bilge("app-test");
    
    public void Foo() {  
        // Use Instance
        b.Info.Flow();
    }
}
```

If you prefer not to create separate instances of Bilge then you can use an inbuilt static instance called default, but if you do this then you loose the additional context that can be provided in the constructor.  The default instance is useful any time that you have the need to add trace to a limited area of the system or don't want a Bilge instance.

```csharp
Bilge.Default.Info.Log("Default Instance");
```

### Log Levels
Once you have an instance of Bilge you need to use logging statements, the statements are operated at different levels.

Each log statement that you write will need to be at a specific logging level - Verbose, Info, Warning and Error.  Typically there should be a lot more Verbose logs than Informational, more Informational than Warning and as few Error logs as you can.  This ensures that as more detailed logging is captured it can be turned on incrementally to minimize the performance overhead of the logging.  To log a message at each level use the following code:

```csharp

b.Verbose.Log("Hellow Cruel Verbose World");
b.Info.Log("Hello Cruel Informational World");
b.Warning.Log("Hello Cruel Warning World");
b.Error.Log("Hello Cruel Error World");
```


### Basic Logging - using Flow.

Very often you will simply want to know where you are in the code - you often find yourself writing trace statements such as this

```csharp
public void foo(string bar) {
    b.Info.Log("In method foo");
}
```
This is such a common approach that there is a special method for it - the Flow method should be used like this:

```csharp
b.Info.Flow();
```

As this is used so commonly you may also want to specify the parameters to the method:


```csharp
public void foo(string bar) {
    b.Info.Flow($"{bar}");
}

```


### Basic Logging - Using Log.

Log is the default logging method and is used for almost all logging.  

```csharp
Bilge b = new Bilge();
b.Info.Log("Simple Statement");
b.Info.Log("First Message", "Supporting Information");
```

One or two strings can be provided, typically the second string holds considerably more information or details related to the first logging string.  For file system based listeners this rarely makes any difference but when loaded into FlimFlam the secondary informaiton is displayed as secondary information to the main trace entries.


### Dump Logging - Using Dump.

Dump will attempt to dump the contents to the trace stream.  If an exception is passed then it will be dumped as an detailed exception looking for inner exceptions and building up the exception tree.   If an array or list is passed Dump will attempt to iterate through the different items writing each one to the trace stream.

```csharp
b.Info.Dump(anobject, "Context");
```

### Enter and Exit Logging - Using E and X and EnterSection and ExitSection.

You can capture flow through methods and program sections with special trace indicators to indicate the flow through code and specific sections. This is done using the following methods:

```csharp
public void Foo() {
    b.Info.E();    // Pushes Foo onto Method Stack
    // Method Code
    b.Info.X();    // Pops Foo off method stack
}
```

You can also mark areas of the code as sections, sections are named and are per thread.

```csharp
public void Foo() {
    
    b.Info.EnterSection("Repository Code");
    // Your Code
    b.Info.LeaveSection("Repository Code");
}
```


### Timing Areas of Code using TimeStart and TimeStop

Timing can be done using TimeStart and TimeStop.  The timers have a unique name and a sink to identify them.  Therefore the individual timer is marked by the name and the collection of timers by the sink.  For example you could have a Sink of Database updates, with individual timers for Read / Write and Delete Operations.

As the strings that are passed are how the timer calculations and timings are worked out it is essential that the strings are passed identically into the start and the stop methods.  Ideally use a separate constant for the name.

```csharp
const string UPDATE = "updateDatabase";
const string CATEGORY = "Database";
b.Info.TimeStart(UPDATE, timerCategoryName:CATEGORY);
// Code Here.
b.Info.TimeStop(UPDATE, timerCategoryName:CATEGORY);
```

