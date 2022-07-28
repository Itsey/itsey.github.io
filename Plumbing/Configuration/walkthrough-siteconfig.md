# Plisky.Plumbing -  Walkthrough.

### Example walkthrough adding configuration to an asp core site.


#### Adding in the DB connection string.

Typically you will add a DBContext in the service configuration area of the site - to do this retrieve the connection string from ConfigHub after configuring the Hub to look for settings.

```csharp


// Program.cs the application startup 
public static void Main(string[] args) {
    
    var settingsFilename = $"{settings}_{Environment.GetEnvironmentVariable("ENVID")}";
   
    #if DEBUG
        // Configure environment identifiers separately - using enviornment variable.
        ConfigHub.Current.AddDirectoryFallbackProvider("%PLISKYAPPROOT%\\Config\\", settingsFilename);
    #else
        // This looks for a prod.donotcommit settings file in the application root directory.
        ConfigHub.Current.AddDirectoryFallbackProvider("[APP]", "prod.donotcommit");

    #endif
}


 // From then on settings can be retrieved like this:
 public void ConfigureServices(IServiceCollection services) {
     services.AddControllersWithViews();

     string connectionString = ConfigHub.Current.GetSetting("dbconstr", true);

     services.AddDbContext<TkdDbContext>(options =>
         options.UseSqlServer(connectionString)
     );
```





#### A note on Entity Framework Configuration

If you use entity framework directly for altering the DB, sometimes you will find yourself in a sitaution where the configuration needs to be duplicated.  The simplest way to do this is with a compiler directive and an additional registration in the EF code.


```csharp


 public void ConfigureServices(IServiceCollection services) {
     services.AddControllersWithViews();

     string connectionString;
            
#if DEBUG && false
     // This is used for Entity Framework configuration only, when EF is generating migrations it doesnt call the main startup.
     try {
         connectionString = ConfigHub.Current.GetSetting("dbconstr", true);
     } catch (Exception) {
         ConfigHub.Current.AddDirectoryFallbackProvider("%PLISKYAPPROOT%\\Config\\", "tkd1100.settings");
         connectionString = ConfigHub.Current.GetSetting("dbconstr", true);
     }
#else
         
     connectionString = ConfigHub.Current.GetSetting("dbconstr", true);
#endif

     services.AddDbContext<TkdDbContext>(options =>
         options.UseSqlServer(connectionString)
     );
```