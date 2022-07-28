### Bilge Web Extensions (Plisky.Diagnostics.Web)

Bilge

#### Basic Logging

Basic logging is what you will use most of the time.  Bilge provides Info, Verbose, Warning and Error logging see the following documentation:
[Basic Logging](basiclogging.md)

#### Dump Logging

#### Tracing Program Flow

Quite often you will find yourself writing code like Log("Im in methodname") and this tends to get moire frequent as you hunt for errors.  Bilge provides flow to track this more easily - see [Tracing Program Flow](diags-bilge-usingflow.md)

### Transient Data

Occasionally you need data that transitions very quickly - there is a special implementation of transient data in Bilge which can be represented in flimflam.  It is not a common scenario - see [Using Transient Data](diags-bilge-transientLogging.md)

#### Actions
Actions are a specific use case - there are times when you need to know whether the code has done something, either for the point of view of testing or to be sure features are working as expected.  
#### Utilities