## Versioning Pages.

## Versioning Pages Navigation.

[Home](version-index.md) | [Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## Using The Versioning In Scripts - e.g. Powershell

Sometimes you will want to use the version number that is generated in a script, rather than replace in a predefined file.  There are several output methods
that are used to do this.


### Walk-Through - Using Semantic versioning for a pre=release.

Semantic versioning would use three digits as the primary version number then potentially a series of other digits to describe pre-release versions.  In this example we will use a three digit format followed by a release name and then a two digit format.  The versioning sequence would then be:

|Starting Version|Type Of Release|Release Version|
|----------------|---------------|-----------------|
|1.0.0-Alpha.1.0 | Production    | 1.0.0           |
|1.0.1-Alpha.1.0 | Production    | 1.0.1           |
|1.0.1-Alpha.1.0 | Pre           | 1.0.2-Alpha-1.0 |
|1.0.1-Alpha.1.1 | Pre           | 1.0.2-Alpha-1.1 |
|1.0.2-Alpha.1.55 | Production   | 1.0.2 |

This sequence increments the third digit of the release number each time a production release is made.  This way the next pre-release is always building up to the next release digit.  

// TODO : Complete This Walkthrough.

### Walk-through - Using Versioning in a Docker Tag

This assumes that you have already created a version store and are using it to version elements such as the code.  For simplicity none of that will be included in this guide and it will also be assumed that the version store will be incremented elsewhere ( for example during the code build process). Therefore this walk-through shows how to tag a docker image with the same version number that was just applied to the code.

Take this PowerShell script:

```dockerfile
docker build -t papi-api .
docker tag papi-api itseyreg.azurecr.io/papi-api:latest
```

One way to tag the version is by doing a replacement on the file and treating it as a text file like this:

```dockerfile
docker tag papi-api itseyreg.azurecr.io/papi-api:XXX-VERSION-XXX
```

This works but requires that you update your script file each time, and that is not the most convenient if running by hand, therefore we will look at an alternative approach that can be run inline to the file to take the correct version.

```dos
versonify --command=passive --version-source=C:\temp\aversion.vstore --output=file
$verval = Get-Content plisky-version.txt
docker tag papi-api itseyreg.azurecr.io/papi-api:$verval
```

The passive mode of operation does no increment but simply opens the versioning store and retrieves the version number. The `--output=file` option then writes that to a file. This can then be read by your own utilities or, in this case, the `Get-Content` command in PowerShell.

Once the value is in a variable in powershell it can be used as normal.
