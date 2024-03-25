## Nuke notes.


Run nuke :Setup from the directory where the sln is using --root


```
    [Solution]
    readonly Solution Solution;

    AbsolutePath SourceDirectory => RootDirectory / "src";

    Target Compile => _ => _        
        .Triggers(UnitTest)
        .Executes(() => {
            DotNetTasks.DotNetBuild(s => s
               .SetProjectFile(Solution)
               .SetConfiguration(Configuration)
               .EnableNoRestore()
               .SetDeterministic(IsServerBuild)
               .SetContinuousIntegrationBuild(IsServerBuild)
           );

        });
```




```
 Target UnitTest => _ => _
     .After(Compile)
     .Executes(() => {

         var testProjects = Solution.Projects.Where(x => x.Name.EndsWith(".Test")).ToArray();
         if (testProjects.Any()) {
             DotNetTasks.DotNetTest(s => s
                 .EnableNoRestore()
                 .EnableNoBuild()
                 .SetProjectFile(testProjects.First().Directory));               
         }



     });
```
