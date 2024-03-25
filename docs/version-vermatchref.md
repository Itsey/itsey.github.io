### Versioning Format Identifiers



#### TextFile Version Behaviour

Any file where literal replacement takes place within the file, using well known tokens:

| Token               | Versioning Style       | Example |
|---------------------|------------------------|---------|
| XXX-RELEASENAME-XXX | The release name  | Unicorn |
| XXX-VERSION-XXX     | The Short Version | 1.2     |
| XXX-VERSION3-XXX    | The 3 digit version | 1.2.3 |
| XXX-VERSION4-XXX    | The 4 digit version number | 1.2.3.4 |




##### Display Type Short
Two digit display.  E.g. 0.0 - The following file types default to this display method:

NetAssembly, TextFile, StdAssembly

##### DisplayType Full
Four digit display E.g. 0.0.0.0 - The following file types default to this display method:  

NetFile, NetInformational, Wix, StdFile, StdInformational

##### DisplayType Three Digit
Three didgit display - E.g.  0.0.0 - The following file types default to this display method:

Nuspec


Wix 

Wix Setup file, looking for the version attribute.  To update the version in the name too, use the text file version as well.

Nuspec

Nuget Package File format.

>Tip: You can specify more than one version type for the same file, therefore you can update a textual element of a Wix file and the version element by using a TextFile format and a Wix format.
```
e.g. in autoversion.txt 

**/product.wxs|Wix
**/product.wxs|TextFile
```
