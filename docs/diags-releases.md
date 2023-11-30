# Plisky.Diagnostics & Plisky.Diagnostics.Handlers Releases.

### Diagnostics NEXT

* 🆕 Changed default from adding timestamps false to true. Messages now default to having timestamps.

Previously timestamps were not included for most messages  (some handlers added them), but you could always configure Bilge to add timestamps, this was done as a form of optimization in that most of the time, in dev, timestamps are not useful.  However this led to a failure to include timestamps in live configuration so switched the default to ensure that timestamps are on.

### Handlers NEXT

* 🐛 Bug in the formatter for the console handler meant that blank lines were being left, this has been removed.

### Diagnostics 3.2.0 

Changes to the pipelines to support the move to GitHub for 3.1.13 meant the versioning was broken, this has now been updated and 3.2.0 is a non functional change to reset at a known point where the versioning issues are resolved.  3.1.13 removed from public nuget.

#### Diagnostics  3.1.13  (Removed)

* 🆕 Alert now includes a Guid to ensure clear only happens once in flimflam.
* 🆕 Writer.More methods added, to add supplementary logging information to an existing log message.
* 🆕 Much more context is now passed to the listeners, including all of the associate tags
* 🐛 <a href="https://github.com/Itsey/plisky-diagnostics/issues/2" target="_blank">Issue #2.</a> ⚠INTERFACE BREAK - Dynamic removed from log statements.  Caller attributes were not longer working with dynamic 

Primarily used this release to move to Github for all code, previously was a combination of azure DevOps and Github but have now moved the code over to only be in Github.  Also fixed a bug where adding dynamic overloads to the methods had broken the use of callermembername and line and file attributes.  Therefore removed dynamic from the log interfaces and created a More option to specify additional logging if required.

#### Diagnostics 3.1.12
* 🆕 Added error capturing for error numbers.  

#### Diagnostics 3.1.11
* 🆕 Added structured error logging to capture specific properties.

#### Diagnostics 3.1.10
Non code related change, moving code to GitHub.

#### Diagnostics 3.1.9
* 🆕 Added structured logging capabilities.
* 🆕 Added error number reporting capabilities.
* 🆕 Added behaviour to assertion approaches.
* 🆕 Switched to multi-targetted package.
* ✂ net5 support removed (standard still present)
 
#### Diagnostics 3.1.6
* ✂ Removed 462,472,48 frameworks in favour of net standard
* 🆕 Added formatter to support format strings
* 🆕 Altered tracemessagetransport to allow support for bilgemiddleware
* 🆕 Added Bilge.SimplifyRouter for Blazer Support.

Simplify router now reduces the router to a single threaded entity, rather than using its own threading as Bilge has done historically, this allows it to play nicely on less powerful platforms such as the in browser experience that Blazor provides.  Bilge can then be used for tracing within a Blazor client app in a browser.

#### Diagnostics 3.1.2
* ✂ Issue with older framework versions in 3.1.0 resolved. (3.1.1/0 Superseded)
* ✂ Removed support for .net framework 4.0 ( 4.5.2 still present)

 
#### Diagnostics 3.1.0
* 🆕 Bilge.Default added to allow for quick tracing in things like properties where creating an instance feels wrong.
* 🐛 Flush did not always flush, especially if the listeners were really slow.  Flush should now be more reliable.
* 🐛 Double new lines were being written out by some listeners.  Formatter now has the responsibility of newlines alone.
* 🆕 (Listeners) RollingFileSystemHandler added to support continual logging


#### Diagnostics 3.0.0
* 🆕 Major Version Uplift -  Actions Added.
* 🆕 Actions - Allow hooks on activities performed in code to support unit testing
* 🆕 Allow use of Formatters on all default handlers.  New FlimFlam2 Formatter added.
* 🆕 Alert now returns a string with a summary title that can be used in other logging.
* 🆕 Support for 462, 472, 48 Framework Versions Added.
* 🐛 Bug - Correct information on trace level written to listeners for Error, Log, Verbose.


Actions abuse the fact that trace is pervasive in the code to give hooks into different parts of the code, this can be useful for unit testing or validating things at different levels of the code base without compromising an otherwise clean structure.  Depending on how you feel about this its either a neat use of an existing feature in the code or a misuse of a cross cutting concern.
      
#### Diagnostics 2.8.6
* 🆕 Added Alerting level, 
* 🆕 AppOnline 
* 🆕 Added Init From File and environment variable. 
* 🆕 Statics for configuring and adding handlers (reduce duplicates etc)

#### Diagnostics 2.8.2
* 🐛 X Flush Fix, flush only worked under load.

### Handlers 2.8.2
* 🆕 Adding new FileSystemHandler

#### Diagnostics 2.8.1    
* 🐛 Listeners Fix 
* 🐛 SimpleFileListener Fix
* ✂  2.8.0 Superceeded.

### Handlers 2.8.1
* ✂ Removing broken FileSystemHandler

#### Diagnostics 2.8.0 
* 🆕 ⚠INTERFACE BREAK ⚠TraceLevel is now SourceLevels, new Feature Bilge.SetConfigurationResolver allows SourceSwitches and TraceSwitches.

### Handlers 2.8.0
* 🆕 Compatibility with new feature release for Bilge, Added skeleton DebugTCPHandler.

#### Diagnostics 2.7.2
* 🐛 Missing method exception issues resolved  (Critical fix for 2.7.X and 2.6.X)

#### Diagnostics 2.7.0
* 🐛 2.7.0 - Utter Mare on constructors, changing interface.


### Handlers 2.7.0
* 🆕 Missing method exception issues.
* 🆕 Remove signing accross Plisky
* 🆕 Harmonised versioning across all libraries.
* 🆕 Bugfix for Net40 support in both Bilge and Listeners.
* 🆕 Moving Legacy Formatter to Bilge not Handlers to ease handler development.
* 🆕 Adding Github Project Url.


