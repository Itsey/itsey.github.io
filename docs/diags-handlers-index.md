Plisky.Diagnostics.Listeners ships with several default handlers which deal with the output of the trace messages, but you can also add your own custom listeners if you so wish.

Each listener uses a Formatter to determine the format of the output, the most common formatters are
* LegacyFlimFlamFormatter - Used for compatibility with FlimFlam1
* FlimFlamV2Formatter - Used for future compatibility with FlimFlam2
* PrettyFormatter - Used when the content will be read by people
* CustomOutputFormatter - Used with your own format string

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


## SimpleTraceFileHandler
Default Formatter: PrettyFormatter

SimpleTraceFileHandler is a very lightweight handler that writes to a well known file name in your temporary directory.  You can configure whether the file is overwritten each time using a parameter to the constructor. It is not designed as a production handler.
SimpleTraceFileHandler is the only handler that is shipped inside the Plisky.Diagnostics assembly, all other handlers are shipped in Plisky.Diagnostics.Listeners.

### Formatters

All of the handlers use a formatter to convert the message type into the output string, each one has a default but you can change this with SetFormatter.



#### CustomOutputFormatter

```
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

New lines will be appended based on the options in MessagEFormatterOptions.  The Message Type will use the default conversion of a message type to string within Bilge but this can be overridden by setting the function MessageTypeConvert which takes in the enum of message types and returns a string.