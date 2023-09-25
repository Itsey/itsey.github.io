# Plisky.Diagnostics Releases.

#### 3.1.13

Non code related change, moved to GitHub for issues.

* 🆕 Alert now includes a Guid to ensure clear only happens once in flimflam.
* 🆕 Writer.More methods added, to add supplementary logging information to an existing log message.
* 🆕 Much more context is now passed to the listeners, including all of the associate tags
* 🐛 <a href="https://github.com/Itsey/plisky-diagnostics/issues/2" target="_blank">Issue #2.</a> ⚠INTERFACE BREAK - Dynamic removed from log statements.  Caller attributes were not longer working with dynamic 

#### 3.1.12
* 🆕 Added error capturing for error numbers.  

#### 3.1.11
* 🆕 Added structured error logging to capture specific properties.

#### 3.1.10
Non code related change, moving code to GitHub.

#### 3.1.0
* 🆕 Bilge.Default added to allow for quick tracing in things like properties where creating an instance feels wrong.
* 🐛 Flush did not always flush, especially if the listeners were really slow.  Flush should now be more reliable.
* 🐛 Double new lines were being written out by some listeners.  Formatter now has the responsibility of newlines alone.
* 🆕 (Listeners) RollingFileSystemHandler added to support continual logging


#### 3.0.0    
* 🆕 Major Version Uplift -  Actions Added.
* 🆕 Actions - Allow hooks on activities performed in code to support unit testing
* 🆕 Allow use of Formatters on all default handlers.  New FlimFlam2 Formatter added.
* 🆕 Alert now returns a string with a summary title that can be used in other logging.
* 🆕 Support for 462, 472, 48 Framework Versions Added.
* 🐛 Bug - Correct information on trace level written to listeners for Error, Log, Verbose.
      
#### 2.8.6
* 🆕 Added Alerting level, 
* 🆕 AppOnline 
* 🆕 Added Init From File and environment variable. 
* 🆕 Statics for configuring and adding handlers (reduce duplicates etc)

#### 2.8.2
* 🐛 X Flush Fix, flush only worked under load.

#### 2.8.1    
* 🆕 Listeners Fix, SimpleFileListener Fix 2.8.0 Superceeded.

#### 2.8.0 
* 🆕 ⚠INTERFACE BREAK ⚠TraceLevel is now SourceLevels, new Feature Bilge.SetConfigurationResolver allows SourceSwitches and TraceSwitches.

#### 2.7.2
* 🐛 Missing method exception issues resolved  (Critical fix for 2.7.X and 2.6.X)

#### 2.7.0
* 🐛 2.7.0 - Utter Mare on constructors, changing interface.