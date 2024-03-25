### Mollycoddle Nuke Build


### Plisky.Nuke.Fusion package

To use Mollycoddle in a nuke build include the Plisky.Nuke.Fusion package, this will provde access to the tasks below for Mollycoddle.

A typical molly scan task looks like this

``` csharp

    Target MollyCheck => _ => _
       .DependsOn(Initialise)
       .Before(Precheck)
       .After(Clean)
       .Executes(() => {
           MollycoddleTasks.PerformScan(s => s
               .AddRuleHelp(true)
               .SetRulesFile(@"<pathtorilesfile>\XXVERSIONNAMEXX\defaultrules.mollyset")
               .SetPrimaryRoot(@"<pathtoprimaryfiles>")
               .SetDirectory(GitRepository.LocalDirectory)
       });

```

You will usually target the GitRepository directory for the root of the scan.  The tasks include a version name of "default" by default therefore xxversionnamexx will be replaced by default.

If there are no violations the log will show in the nuke log as follows:

![Molly Passing](assets/images/nuke-molly-pass.png)


All of the molly command line options are supported.