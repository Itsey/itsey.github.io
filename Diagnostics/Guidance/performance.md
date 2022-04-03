
### Analyzing Performance  - Walk through

In this example we use the trace in an application to determine the performance issues surrounding a page render and the improvement that the changes make.  Bilge performance is quick and dirty, it is not a substitute for a performance analysis tool but quite often it is enough to get you to the point where detailed performance analysis can support code changes.

From the initial view we can see that the page renders and then a large number of retrievals for tags are performed, there are two things that stand out from the trace - #1 ActualGetTagsForTargetId is hit repeatedly and #2 the full 30 images are queried and then only 10 are returned.  As the number of images grows this will perform worse and worse.

First thing is add some timing code
```
public ActionResult Gallery(string Id = null) {
    b.Info.Flow($"ID: {Id}");
    b.Verbose.TimeStart("GalleryController", timerCategoryName: "GalleryEndToEnd");
    // Rest of the controller code goes here
    b.Verbose.TimeStop("GalleryController", timerCategoryName: "GalleryEndToEnd");
    return View(vm);
}

```

Time starts and stops must have identical titles to link up correctly and this will then show in Flimflam as a display of the timings. We can already see that the majority of the time is spent in the GetGallery method.