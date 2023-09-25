### Using Error Reporting

You can optionally use Bilge to capture error numbers and report on them.  This is utilising the fact that the trace library is pervasive in code for other cross cutting concerns and therefore might not gel with the way that you prefer to structure your code.


#### Recording Errors

You can record error numbers, this ensures that an internal store of error numbers is registered.  This can then be used to validate that the error numbers have all been recorded.  This is useful to ensure that you have a correct and comprehensive set of error numbers recorded.  It is most useful when combined with conditional compilation to ensure that the excess code is not present in release builds.


#### Reporting Errors

```csharp
int res = blr.Error.Record(new ErrorDescription(0x0001, 0x0001, "Access Denied"));

res = blr.Error.ReportRecord(new ErrorDescription(0x0001, 0x0001, "Access Denied"), "Context");
var ad = new Exception("Access Denied") {
    HResult = res,
};


```
