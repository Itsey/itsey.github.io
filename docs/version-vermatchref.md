## Versioning Pages Navigation.

[Home](version-index.md) |[Command Line](version-commandline.md) | [Quick Start](version-quickstart.md) | [Overview](version-overview.md) | [Reference](version-reference.md) |  [Nuke](version-nuke-quickstart.md)

### Versioning Format Identifiers

#### TextFile Version Behaviour

Any file where literal replacement takes place within the file, using well known tokens:

| Token               | Versioning Style           | Example       |
| ------------------- | -------------------------- | ------------- |
| XXX-RELEASENAME-XXX | The release name           | Unicorn       |
| XXX-VERSION-XXX     | The Full Version           | 1.2.3-pre.0.0 |
| XXX-VERSION2-XXX    | The Short Version          | 1.2           |
| XXX-VERSION3-XXX    | The 3 digit numeric version| 1.2.3         |
| XXX-VERSION4-XXX    | The 4 digit numeric version| 1.2.3.4       |

### Versioning Display Types

> Tip: You can specify more than one version type for the same file, therefore you can update a textual element of a Wix file and the version element by using a TextFile format and a Wix format.

```
e.g. in autoversion.txt 

**/product.wxs|Wix
**/product.wxs|TextFile
```

#### Display Type Short

Two digit display.  E.g. 0.0 - The following file types default to this display method:

- TextFile

#### Display Type Full

Full digit display. E.g. 0.0.0.0 - The following file types default to this display method:

- NetInformational
- Wix
- Nuspec
- StdInformational

#### Display Type Three Digit

Three digit display. E.g. 0.0.0 - The following file types default to this display method:

- Wix
- Wix Setup file, looking for the version attribute.  To update the version in the name too, use the text file version as well.
- Nuspec
- Nuget Package File format.

#### Display Type Four Digit

Four digit display. E.g. 0.0.0.0

#### Display Type Four Digit Numeric

Four digit numeric display will display the value of the first four digits in the version store, using a dot separator. E.g. 0.0.0.0 - Only integer values will be displayed. 
If a non-integer digit value is found, this will be automatically replaced with a '0' along with every subsequent digit, ensuring the version remains in a valid numeric format. 
The following file types default to this display method:

- NetAssembly
- NetFile
- StdAssembly
- StdFile

#### Display Type Three Digit Numeric

Three digit numeric display will display the value of the first three digits in the version store, using a dot separator. E.g. 0.0.0 - Only integer values will be displayed.
If a non-integer digit value is found, this will be automatically replaced with a '0' along with every subsequent digit, ensuring the version remains in a valid numeric format.

#### Display Type Queued Full

Displays the full version number with any queued values applied.
E.g. if the version store is "1.0.0.0" and the queued value for the first digit is "2", the displayed version will be "2.0.0.0"