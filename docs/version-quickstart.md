## Versioning Pages Navigation.

[Home](version-index.md) |[Command Line](version-commandline.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

## Versioning Quick Start

### Step 1

**Create** the storage file which contains the versioning number that you are going to use.  The new file will default to 0.0.0.0 and a fixed behaviour scheme.

```dos
versonify --command=createversion --version-source=C:\temp\myappname.vstore
```

### Step 2

**Configure** your source repository with a text file describing which files you want to apply versioning to.  This will contain a list of version minmatches.  

Create a file like one below and save it as autoversion.txt in your repository.This is a set of minmatchers and you should match your code (for example the convention below has source code in a /src folder)

```plaintext
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetAssembly
**/src/**/CommonAssembly*.cs|NetFile
**/src/**/CommonAssembly*.cs|NetInformational
**/src/**/*.csproj|StdAssembly
**/src/**/*.csproj|StdInformational
**/src/**/*.txt|TextFile
```

### Step 3

**Increment** the version number and apply the changes to your source files with the UpdateFiles command.  

```dos
versonify --command=updatefiles --root=.\LibSrc\ --version-source=\\server\versionFname.vstore --increment --min-match=AutoVersion.txt
```

### Step 4

**Investigate** what else you can do - add step 3 to your build pipeline, include it in batch files, read the documentation etc.