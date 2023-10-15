### Versioning Format Identifiers

NetAssembly, NetFile, NetInfomrational

Net framework based cs files looking for the corresponding attribute in the file to apply the version.

StdAssembly, StdInformational, StdFile

Net standard (csproj file) looking for properties in a property group to apply the version.


#### TextFile

Any file where literal replacement takes place within the file, using well known tokens:

| Token               | Versioning Style       | Example |
|---------------------|------------------------|---------|
| XXX-RELEASENAME-XXX | The release name  | Unicorn |
| XXX-VERSION-XXX     | The Short Version | 1.2     |
| XXX-VERSION3-XXX    | The 3 digit version | 1.2.3 |
| XXX-VERSION4-XXX    | The 4 digit version number | 1.2.3.4 |






Wix 

Wix Setup file, looking for the version attribute.  To update the version in the name too, use the text file version as well.

Nuspec

Nuget Package File format.
