# How To Create A Check-list.

## Pre-requisites

* New machine 


## Steps

[step]:Step10;#Beginner;#Advanced;#Explanation

#### Step - 10 - Disable W11 Show more options.

```cmd
::Disable
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

::Enable
reg delete "HKEY_CURRENT_USER\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f​
```
