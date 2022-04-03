## Plisky.Code Craft.   

Plisky.Code Craft is a series of libraries, tools and practices all around writing better software.  This module is a versioning tool designed to consistently apply and work with version numbers for Wintel stack code.

## Versioning Reference.

### Versioning Reference Types

.Net Framework identifiers
+ NetAssembly
+ NetFile
+ NetInfomrational

.Net Core Identifiers

+ StdAssembly   ( Assembly Version )
+ StdInformational   (Informational Version)
+ StdFile (File Version)

Other Identifiers

+ TextFile  (Plain text substitution for XXX-VERSION-XXX)
+ Wix ( Wix setup XML format)
+ Nuspec ( Nuget package format )


### Behaviours

The versioning increment is based on the behaviour of the digit.  


#### Fixed (0)
Fixed values do not change.  They remain constant throughout a version increment.

####  MajorDeterminesVersionNumber (1)
Not Implemented.


#### DaysSinceDate (2)
DaysSinceDate will reflect the number of days that have elapsed since the BaseDate

#### DailyAutoIncrement (3)
DailyAutoIncrement will increment each time that the increment is called for the current build date.  Therefore multiple builds on the same day will have incremental versions but the next day the number resets to zero.

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
