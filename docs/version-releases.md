
## Versioning Pages Navigation.

[Home](version-index.md) | [Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## 💖 Versonify Release History💖.



⬆️ 1.0.3 - Austen Compatibility Release.
  - ✅ Feature - Implememted --QQpnf quick version return exit codes to tell PNF what compatibility version Versonify is running.
  - ✅ Feature - Implemented -z to suppress non zero return exit codes.
  - ✅ Feature - 💥Breaking Change💥 File Updates that do not update any files now default to returning non zero exit code.  Add -z for old functionality.
  
    Compatibility release gives a command line response with a compatibility code to tell calling scripts which version of versonify is being used and therefore what capabilities are supported.  First compatibility version code is 200 which can be returned by calling versonify --QQpnf.  This is the only parameter required for the compatibility code which is returned as the exit code.
    
    Default is now that a non zero exit code is returned if there are no files updated with the update files command or if the MM is missing.  These defaults mean that its much more obvious now if Versonify is doing nothing.  There is now a -z switch to allow for backwards compatibility of returning zero when no updates are made.
  
⬆️ 1.0.1 - Austen.
  - First Release ( Austen Release ).
  - Behaviour Update Added.
  
  Austen release, first v1 release with most features implemented.  Nexus and SMB fileshare support for vstores as well as increments and file updates implemented.

⬆️ 0.2.0 - Austen Pre-Release.
  - Moved to being a dotnet tool.
  - Now supports display of behaviours.  ( -Command=behaviour)
  
  Austen Pre-Release moved to being a .net tools package so now it is no longer referenced directly by the project in nuget but referenced as a tools package.  Can also be installed as a global tool if required.   
  
⬆️ 0.1.3 - Initial.
  - 🐞 Critical Bug - Typo in exe name in package corrected.  0.1.2. Superceeded.
  
⬆️  0.1.2 - Initial.
  - Added Nexus Support.      
  
⬆️  0.1.1 - Initial.
  - Updated documentation to remove references to PliskyTool.
  - 🐞 Fix - DryRun no longer updates the files on disk.

⬆️ 0.0.0 - Initial.
  - Renamed to become versonify.
  - Released as tool package to support nuke.

