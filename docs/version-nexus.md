### ## Using Nexus As A Repository.

Mollycoddle and Versonify both require datafiles to work there are currently three supported storage mechanisms.  You can use a local path on your machine, a network share using \\\server notation or Nexus artefact repository.  Using nexus is a simple way to have centralised storage of version numbers, molly rules and primary files. To use nexus you must use a path that starts with [NEXUS].

```text
Example Nexus URL:
[NEXUS][U::a123$[P::##@@##[L::http://SERVERNAME:8081/repository/plisky/molly/default/defaultrules.mollyset

Specify the Nexus Username Like this
[U::username123
Specify the Nexus Password Like this
[P::password123 
Specify the full url to the file like this
[L::http://SERVERURL/repository/plisky/molly/default/defaultrules.mollyset
Specify the repository id like this
[R::repository
```

For Versonify the nexus initialisation string needs to point to a full version store  file.   The rest of the path can be anything you like.

```text
SERVERNAME
```

For Mollycoddle the rules file needs to point to a full mollyset or rules file, whereas the primary files directory should point to a folder in the repository.  Versioning is supported as usual.

```text
A mollycoddle primary file path
[NEXUS][U::xxx[P::yyyy[L::http://SERVERNAME:8081/repository/plisky/primaryfiles/default/

A prmiary files path which supports versioning
[NEXUS][U::xxx[P::yyyy[L::http://SERVERNAME:8081/repository/plisky/primaryfiles/XXVERISONXX/
```
