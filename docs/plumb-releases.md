Plisky.Plumbing - Release Note.
===========================================

Release notes for the Major version and Major Version - 1 are in the release note part of the Nuget Package.  All historical release notes are here.
### 1.8.0 
  Bug fix for when case does not match a settings name.  Plumbing used to try and parse the first value that it found.    
  Updated to use 3.X version of Bilge, removed injection for Bilge as now Bilge supports global configuration.    
  Added net 5.0 and 6.0 support directly (standard support is still present.)
  Bug fix, configuration support now only available in .net framework versions, method performs no operation in .net core based applications.

### 1.7.0
Bilge TraceSwitch Added (Version Reset) - Release Notes for 1.5 Now Online.

### 1.6.3    
Updating Dependency Chain on Diagnostics To Keep Up with New Constructor Changes

### 1.5 
Corrects versioning fault in different targeted frameworks, all previous versions deprecated. 
Adds int and string array support to command line arguments.  Comma separated only.
HttpHelper is deprecated
                  
### 1.5.1  
Adds DateTime as a supported format.  Adds required option to attributes.
                
### 1.5.2  
Fixes NullRef Exception when using bilge.

### 1.5.3  
Adds formatting for DateTime parsing.

### 1.5.4  
Adds environment support to directory. 

### 1.5.5  
License Format Update

### 1.5.5  
Adds Bilge Injection to ConfigHub, Hub and CommandLineArguments.

### 1.5.7  
Corrects Issue With 1.5.5 in .net standard.

### 1.5.8  
Removes case insensitivity in configHub, corrects issue wiht [APP] tag.

### 1.5.9  
Adds SSL Cert bypass, corrects net standard versioning.

### 1.5.10 
Fixes SSL Bypass
 
### 1.5.11 
Diagnostics Update for References.

### 1.5.12 
Further Diagnostics update for references

### 1.5.13 
Addition of AsyncProcessSupport and HttpHelper Uri bug fix
    