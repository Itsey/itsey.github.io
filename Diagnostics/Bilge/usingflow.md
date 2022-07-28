### Tracing Program Flow

Tracing program flow is one of the most common activities when trying to diagnose faults in your code, mostly this is what tracing is used for to try and follow the flow of execution that a user takes through your source code to work out where the issue lies.


Tracing program flow with Bilge has multiple different facets and these can get quite complex.


### The basics - using flow.

Very often you wil simply want to know where you are in the code - this is achieved with the flow method

```csharp
b.Info.Flow();
```

As this is used so commonly you may also want to specify the parameters to the method:


```csharp
public void foo(string bar) {
    b.Info.Flow($"{bar}");
}

```

