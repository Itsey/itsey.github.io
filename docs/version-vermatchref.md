### Versioning Format Identifiers

NetAssembly, NetFile, NetInfomrational

Net framework based cs files looking for the corresponding attribute in the file to apply the version.

StdAssembly, StdInformational, StdFile

Net standard (csproj file) looking for properties in a property group to apply the version.

TextFile

Any file, looking for XXX-VERSION-XXX to replace with the version number.

Wix 

Wix Setup file, looking for the version attribute.  To update the version in the name too, use the text file version as well.

Nuspec

Nuget Package File format.
