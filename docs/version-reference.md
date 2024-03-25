## Versioning Pages.

[Home](version-index.md) | [Quick Start](version-quickstart.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)


## Plisky.Code Craft.   

Plisky.Code Craft is a series of libraries, tools and practices all around writing better software.  This module is a versioning tool designed to consistently apply and work with version numbers for Wintel stack code.

## Versioning Reference.

### Versioning Reference Types

Versonify understands particular file formats that it can update with the version number.  The versioning reference types serve as a list of all of the supported file formats.

**.Net Framework identifiers**
+ NetAssembly
+ NetFile
+ NetInformational

**.Net Core Identifiers**

+ StdAssembly   ( Assembly Version )
+ StdInformational   (Informational Version)
+ StdFile (File Version)

**Other Identifiers**

+ TextFile  (Plain text substitution for XXX-VERSION-XXX)
+ Wix ( Wix setup XML format)
+ Nuspec ( Nuget package format )

#### Default Digit Display for Versioning Reference Types.

[To see default display types view version display reference](version-vermatchref.md)


### Behaviours

The versioning increment is based on the behaviour of the digit.  These behaviours will determine what action occurs once an increment is applied to the affected digit.

The versioning store will determine how this is saved but for the file system store its a simple file where you can edit the behaviour digit using the identifiers below.

```
{"Digits":[{"Behaviour":0,"IncrementOverride":null,"Value":"0","PreFix":""},{"Behaviour":0,"IncrementOverride":null,"Value":"0","PreFix":"."},{"Behaviour":0,"IncrementOverride":null,"Value":"0","PreFix":"."},{"Behaviour":0,"IncrementOverride":null,"Value":"0","PreFix":"."}],"DisplayTypes":{"NetAssembly":1,"NetFile":2,"NetInformational":2,"Wix":2,"Nuspec":4,"StdAssembly":1,"StdFile":2,"StdInformational":2,"TextFile":1},"IsDefault":false,"ReleaseName":null}
```

#### Fixed (0)
Fixed values do not change.  They remain constant throughout a version increment and there is no way to change them through increments.

####  MajorDeterminesVersionNumber (1)
Not Implemented.  This functionality was removed in an old version.


#### DaysSinceDate (2)
DaysSinceDate will reflect the number of days that have elapsed since the BaseDate.  To use this a base date must be specified in the version storage system.

#### DailyAutoIncrement (3)
DailyAutoIncrement will increment each time that the increment is called for the current build date.  Therefore multiple builds on the same day will have incremental versions but the next day the number resets to zero.  Typically this is used positionally after a version digit uses the days since date behaviour.

#### AutoIncrementWithReset (4)
AutoIncrementWithReset will increment continually unless the next digit up has changed.  It therefore behaves exactly like continual increment for the major version part.  For the minor version part it will increment until the major changes then it will reset to zero. For the build the build will increment until the minor version changes. Finally for the revision it will increment until the build version changes.
    

#### AutoIncrementWithResetAny (5)

This will increment continually unless any of the higher order digits have changed.  It therefore continually increments until a more significant digit changes then it resets to zero.  Major digits will continually increment.  Minor digits will continue to increment until the major changes.  Build will continue to increment until either Major or Minor changes and finally the revision digit will continue to increment until any of Major / Minor or Build changes.
        
#### ContinualIncrement (6)
        
Continual increment will increment non stop until an overflow occurs. When an overflow occurs the digit is reset to 0.

#### WeeksSinceDate (7)
This will return the number of whole or partial weeks since the base date.

#### ReleaseName (8)

Will set this digit to be the release name as specified in the version.  Release names can change during an increment but are not incremented or decremented as such.  They are set to literal strings.
