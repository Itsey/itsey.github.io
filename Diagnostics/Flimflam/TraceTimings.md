Using FlimFlam for Timings.

When the Timing features of [https://github.com/Itsey/Plisky.Diagnostics/wiki/Recording-Timings.](Bilge) timings can be placed into FlimFlam to help you analyse the performance of your application.  This is a rough guide to performance and is not a substitute for proper performance profiling tools but can be used to get a good grip on where bottlenecks are and where to narrow down your focus for further investigation.

The FlimFlam timings view looks like this:

![](https://github.com/Itsey/Plisky.Documentation/blob/master/WikiDocs/FFImages/FF_Timings.png)



The entries in the list are all of the TimeStart and TimeExit pairs with their elapsed time.  In this example we can see that there are repeated calls to two repositories - fast repository and slow repository.  The entries are colour coded based on their relative timings - and this highlights that the slow repository is indeed slower.


The timing approach relies on matching start and stop entries - where this does not occur either due to a typo in the timing entry name or due to an exception occurring then the mismatched entries are added to the "entries with no corresponding pair" area of the view.

Different areas of the application can use the same name, so that subsystem response can be monitored.

Therefore for this code:
```
 internal static void Run2(Bilge b) {
            string timerName = "sampleRun";
            string sinkName1 = "WebService";
            string sinkName2 = "DataBase";
            Random r = new Random();
            for (int i = 0; i < 100; i++) {
                b.Verbose.TimeStart(timerName, timerCategoryName: sinkName1);
                Thread.Sleep(r.Next(10));
                b.Verbose.TimeStop(timerName, timerCategoryName: sinkName1);

                b.Verbose.TimeStart(timerName, timerCategoryName: sinkName2);
                Thread.Sleep(r.Next(20));
                b.Verbose.TimeStop(timerName, timerCategoryName: sinkName2);
            }
        }
```

The timer display looks like this - rapidly calling out that the DataBase elements of this code are running slower than the WebService elements ( based purely on the names in the code above ).

[[https://github.com/Itsey/Plisky.Documentation/blob/master/WikiDocs/FFImages/FF_Time_Categories.png|alt=FF Image]]

And clicking on the expand will show the individual entires that add up to that top view, again with the colouring highlighting the difference in timings within the individual repeat calls for the same timer.

[[https://github.com/Itsey/Plisky.Documentation/blob/master/WikiDocs/FFImages/FF_Timings_CategoriesExp.png|alt=FF Image]]

