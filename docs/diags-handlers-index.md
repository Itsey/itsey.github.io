Plisky.Diagnostics.Listeners ships with several default handlers which deal with the output of the trace messages, but you can also add your own custom listeners if you so wish.

Each listener uses a Formatter to determine the format of the output, the most common formatters are
* LegacyFlimFlamFormatter - Used for compatibility with FlimFlam1
* FlimFlamV2Formatter - Used for future compatibility with FlimFlam2
* PrettyFormatter - Used when the content will be read by people
* CustomOutputFormatter - Used with your own format string

In addition for anything more complex you can use a custom handler: [see this link](diags-handlers-custom.md)

## TCPHandler
Default Formatter: LegacyFlimFlamFormatter

TCPHandler is the default handler for use with Bilge.  It will stream your trace out over a network connection to an instance of FlimFlam that is running.  This is designed to give real time feedback to developers of how their application is running during development.  
TCPHandler(string,int) takes a string IP address or hostname and a port to connect to.  


## RollingFileSystemHandler
Default Formatter: PrettyFormatter

The rolling file system handler is used to write to files but allow the files to wrap over on date change or maximum file size limit being reached.  This is aimed at a production filestream handler and requires that the process running the trace has the ability to write to the disk where it is configured to log to.

Configure using the RollingFSHandlerOptions.

```csharp
 var mr = new RollingFileSystemHandler(new RollingFSHandlerOptions() {
                Directory = @"C:\Temp\_Deleteme\Lg\",
                FileName = "Log_%dd%mm%yy_%nn.log",
                MaxRollingFileSize = "2mb",
                FilenameIsMask = true
            });
```

Filename masks allow for replacements to be used. %nn is an incrementing single digit ( when a file exists with the same name a digit is appended).   %dd = Day Of The Month   %mm = Month of the year %yy = Year (four digits).   The FilenameIsMask is used to indicate you are using masking characters.  Without this the filename is treated as a literal.  
MaxRollingFileSize supports kb, mb, gb as filesize types.  100mb for example rolls the filesize every hundred megabytes.
All rolls are done after a batched write therefore files can be slightly larger than the size noted here.

## FileSystemHandler
Uses LegacyFlimFlamFormatter

The filesystem handler is the handler used to write trace to a file system, such that it can be read by FlimFlam later or offline.

## InMemoryHandler
Default Formatter: PrettyFormatter

The InMmemoryHandler stores all of the trace information in memory in a data structure.  You can configure the size of the data that it stores in number of trace rows, and you can retrieve this from the handler as long as you maintain a reference to the handler.  This is designed for use in scenarios where you want to optionally enable debugging and then display the trace details within the applications (e.g. for use in Mobile apps).


## Default Single File Handler (SimpleTraceFileHandler)
Default Formatter: PrettyFormatter

SimpleTraceFileHandler is a very lightweight handler that writes to a well known file name in your temporary directory.  You can configure whether the file is overwritten each time using a parameter to the constructor. It is not designed as a production handler.
SimpleTraceFileHandler is the only handler that is shipped inside the Plisky.Diagnostics assembly, all other handlers are shipped in Plisky.Diagnostics.Listeners.


### GCP Google Logging Handler.  (GoogleLoggingHandler)
Default Formatter: JSON Formatter.

The GCP Google Logging handler writes logs to a google logging account using a project id and an application id.  This will use the google logging API to write the events and therefore you must check that you have the correct permissions.  If an error is thrown while writing to the google logging environment then it will usually result in a loss of the message and all further messages for 30seconds.

When creating the handler it requires a ProjectID and a LogId to be passed in to match the corresponding settings in the GCP Cloud Console. 

```csharp
Bilge.AddHandler(new GoogleLoggingHandler(new GoogleLoggingHandlerOptions() {
    ProjectId = "<projectnamegoeshere>",
    LogId = "test-app"
}));
```

Note, in order to write to the logging environment you will need permissions setup.  This will either be from the service account that your application runs as or by manually granting the permissions like this:

```text
gcloud auth application-default set-quota-project <projectnamegoeshere>
```



### Formatters

All of the handlers use a formatter to convert the message type into the output string, each one has a default but you can change this with SetFormatter.  The formatter determines the exact format that the content is written out in, there are several default formatters available.

To set a formatter use SetFormatter prior to adding the handler into Bilge.

```csharp
 var mr = new MyRoller(new RollingFSHandlerOptions() {
     Directory = @"C:\Temp\_Deleteme\Lg\",
     FileName = "Log_%dd%mm%yy.log",
     MaxRollingFileSize = "2mb",
     FilenameIsMask = true
 });
 mr.SetFormatter(new FlimFlamV2Formatter());
 b.AddHandler(mr);

```




#### JsonFormatter

The JSON formatter is the most useful for interacting with other systems, it is only supported on .net CORE calling platforms.    This formatter creates JSON output like this:

```text
{"Body":"context","ClassName":"","CommandType":1,"Context":"default","FileName":"","FurtherDetails":"","Index":29,"LineNumber":"0","MachineName":"CALYPSO","MessageTags":{},"MethodName":"","NetThreadId":"1","OsThreadId":"1","ProcessId":"19512","StructuredData":{"Fred":"Fred","Bob":5,"Structure":"none"},"TimeStamp":null}
{"Body":"context","ClassName":"","CommandType":1,"Context":"default","FileName":"C:\\Files\\Code\\git\\PliskyDiagnostics\\src\\DevConsoleTest\\Program.cs","FurtherDetails":"","Index":30,"LineNumber":"61","MachineName":"CALYPSO","MessageTags":{"plisky.moreinfo":"This is more info","appversion":"1.0.0.0"},"MethodName":"Main","NetThreadId":"1","OsThreadId":"1","ProcessId":"19512","StructuredData":null,"TimeStamp":null}
```

#### LegacyFlimFlamFormatter

The default formatter to work with FlimFlam, this supports the string format required to stream content directly to Flim Flam.

This formatter creates output like this
```text
{[CALYPSO][8656][1][1][C:\PATH\DevConsoleTest\Program.cs][39][::Main]}#LOG#Hello World 0
{[CALYPSO][8656][1][1][C:\PATH\DevConsoleTest\Program.cs][39][::Main]}#LOG#Hello World 1
```

#### PrettyReadableFormatter

A very basic formatter designed to write to files for readability by a person not by a system.  

This formatter creates output like this
```text
17/11/2022 19:21:34 >> Hello World 0 <<|>> @39 in 20160@CALYPSO Thread[1,1] () [LogMessage]
17/11/2022 19:21:34 >> Hello World 1 <<|>> @39 in 20160@CALYPSO Thread[1,1] () [LogMessage]
```

#### CustomOutputFormatter

```csharp
// Default Format String
 public string FormatString { get; set; } = "{9} {0} >> {1} || {2}@{3} in {4}@{5} >> T[{6}-{7}] || {8} || {10}";
```

Index Values
* 0 -  DateTime.Now
* 1 -  Message Body
* 2 -  ClassName
* 3 -  Line Number
* 4 -  Process ID
* 5 -  Machine Name
* 6 -  Thread Identity
* 7 -  Net Thread Identity
* 8 -  Further Details
* 9 -  Message Type
* 10 - New Line

New lines will be appended based on the options in MessageFormatterOptions.  The Message Type will use the default conversion of a message type to string within Bilge but this can be overridden by setting the function MessageTypeConvert which takes in the enum of message types and returns a string.

This default string would create a format like this.
```text
#LOG# 17/11/2022 19:23:43 >> Hello World 0 || @39 in 5984@CALYPSO >> T[1-1] ||  || 
#LOG# 17/11/2022 19:23:43 >> Hello World 1 || @39 in 5984@CALYPSO >> T[1-1] ||  || 
```

However changing the format string you can alter the output, so setting the format string like this:
```csharp
var cf = new CustomOutputFormatter();
cf.FormatString = "{0} - {1}";
mr.SetFormatter(cf);
```

Would result in log entries that look like this:

```text
17/11/2022 19:26:00 - Hello World 0
17/11/2022 19:26:00 - Hello World 1
```


#### Your Own Formatter

For anything more complex you can build your own formatter.   To do this inherit from BaseMessageFormatter and then you can write any code that you like to generate the output logs.  This is a very limited example but you can add any code you like to create the output string.

```csharp
internal class CustomFormatter : BaseMessageFormatter {

    protected override string ActualConvert(MessageMetadata msg) {
        if (msg.StructuredData != null) {
            result = $"A Log:  {msg.Body} {JsonSerializer.Serialize(msg.StructuredData)}}";
        } else {
            result = $"A Log: {msg.Body}";
        }
            
        return result;
    }

}
```

#### FlimFlamV2Formatter

This is an internal formatter designed to work with FlimFlam, you can use it for other purposes but its internal message format is not designed to be readily used and may change at any point.  Its a semi JSON format which uses short names to reduce transport sizes.

This formatter creates output like this:

```text
{ "v":"2","uq":"--uqr--","dt":"07:12:40 17-11-2022","c":"","l":"39","mn":"Main","md":"C:\\PATH\\DevConsoleTest\\Program.cs","al":"::Main","nt":"1","p":"4908","t":"1","man":"CALYPSO","m":"Hello World 0","s":"","mt":"#LOG#" }
{ "v":"2","uq":"--uqr--","dt":"07:12:40 17-11-2022","c":"","l":"39","mn":"Main","md":"C:\\PATH\\DevConsoleTest\\Program.cs","al":"::Main","nt":"1","p":"4908","t":"1","man":"CALYPSO","m":"Hello World 1","s":"","mt":"#LOG#" }

```
