# Isolating Configuration


Sometimes when working with configuration it must contain sensitive information and this should not be commited to source control.  Using environment variables
or files that are set to be ignored by your source control tool you can do this as per the following examples.

## Simple Use Case



1 - Create The File With the Config In it

```xml
<?xml version="1.0" encoding="utf-8"?>
<chub_settings>
<settings>
  <connectionstring>secretDataYouDontWantInSourceControl</connectionstring>
</settings>
</chub_settings>
```


2 - Save This File - C:\MyAppConfigs\appName\devSettings.chConfig
2a - (Optionally) Set An Environment variable to locate the config set MYAPPCONFIG=C:\MyAppConfigs\

3 - Add the ConfigHub File Fallback Provider
3a - (Optionally) Use %MYAPPCONFIG%
' ConfigHub.Current.AddDirectoryFallbackProvider("%MYAPPCONFIG%\\appName\\", "devSettings");

4 - Retrieve The Value

' var s = ConfigHub.Current.GetSetting<string>("testvalue");


With this approach different settings can be applied on different machines outside the deployment path to keep the configuration constant.


### Using ignored files.

Using files with a consistent .donotcommit extension then including them in your SCM ignore file will allow you to have configuration data in the source path
 that contains sensitive info.  This does present a risk of loosing this information but that is better than having secrets compromised.

```xml
<?xml version="1.0" encoding="utf-8"?>
<chub_settings>
<settings>
  <connectionstring>secretDataYouDontWantInSourceControl</connectionstring>
</settings>
</chub_settings>
```


