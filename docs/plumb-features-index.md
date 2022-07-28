## Using Plisky Feature Support

Features are switches that are designed to allow you to work with conditional code to enable and disable elements of your code through configuration.  

They are  a key enabler for working with a single code branch but can also be used to dynamically configure elements of your 
code to allow for experimentation, separating features from deployments and time based functionality.  

### Concepts

Features are created by a feature provider.  Feature providers have the job of retrieving the feature configuration from some
storage mechanism. There are some feature providers implemented as a part of Plisky.Plumbing but anything that implements IResolveFeatures
can be used to provide features back to the feature framework.  That way you can use whichever configuration you prefer.

Features themselves are created by the provider and deliver basic functionality and caching.  The underlying provider is only
called when necessary as often this will require a storage or network hit.

``` csharp
var f = new Feature("TEST",true);
for(int i=0; i<10; i++) { 
    if (f.Active) {
        Console.WriteLine("Active");
    }
}
// Despite 10 calls only one call to resolve the feature was made.
```

This can be overridden by calling IsActive() which will always force a call back to the feature provider to determine whether
the feature is active or not. This can be useful when doing Red Green testing or any form of feature enabling that requires dynamic configuration.

### Enabling Features

Features are provided through a FeatureProvider which implements IResolveFeatures.  The simplest Feature Provider is a hard coded feature provider where 
all of the features to be resolved are hard coded at app startup.

There are two aspects to the feature provisioning, the initial setup and configuration of a feature provider and then
the use of the features in the code base.

```csharp
//  During applicaiton configuration or setup
var f = new FeatureHardCodedProvider();
f.AddFeature(new Feature("TEST", true));
Feature.AddProvider(f);
```

Consuming the features in the code base is done with Feature.GetFeatureByName() - this retireves a feature instance that can
be tested for being active.


```csharp
// In the area of the code that is consuming the feature.
if (Feature.GetFeatureByName("TEST").Active) {
   // This feature is active thanks to the hard coded feature resolver
}
```

### Simplest UseCase.

```csharp
//  During applicaiton configuration or setup
var f = new FeatureHardCodedProvider();
f.AddFeature(new Feature("WRITELINE", true));
Feature.AddProvider(f);
if (Feature.GetFeatureByName("WRITELINE").Active) {
Console.WriteLine("Feature Active"
}
```

### Features Configured By Date

Features can have a start date, an end date or both:

```csharp
var f = new Feature("CHRISTMAS",false);
f.SetDateRange(new DateTime(2020,12,24), new DateTime(2020,12,26));
```

This enables a feature for three days starting on the 24th December 2020 and ending at the end of the 26th December 2020.  Quite
often you'll want the feature to reoccur each year - to do this specify the final agnostic parameter as true.

```csharp
f.SetDateRange(new DateTime(2020,12,24), new DateTime(2020,12,26),true);
```

Now this feature will be active for three days each year - displaying a Christmas banner or information related to the Christmas period
without any further code changes required.


### Features With Levels

Features can be set to different values, not simply true or false.  Using levels allows you to retrieve the value for a feature but
also use Active to determine if it has any value.  Active will return true if the feature level is anything other than zero.

 


