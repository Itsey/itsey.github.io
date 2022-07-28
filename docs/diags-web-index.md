### Plisky.Diagnostics.Web

Web support for Pliksy.Diagnostics

#### Middleware

Bilge Middlware can be used to provide web enabled services from within an ASP.net core website.  Features include trace redirection, basic response, and dynamic configuration.

The extension UseBilge will allow you to configure Bilge middleware, most of the parameters are defaulted but the one feature you should determine whether to enable manually or not is trace redirection.  If this is set to true then clients can write trace into this servers trace, if false they can not.

```
// In program.cs
  var app = builder.Build();
  app.UseBilge(true);
```

#### Middleware Features

##### Trace Redirection

```
post <base>/bilge-api/bnc/
body [ Array Of MessageMetadataTransport Objects ]
```
Trace will allow a series of JSON serialised MessageMetadataTransport objects to be written directly into the hosting environments trace stream.  This will allow fully formatted bilge statements to be added directly into the server trace logs.

```
post <base>/bilge-api/bnr/
body series of strings CR-LF separated
```

Trace will allow a series of basic strings to be written into the hosting environments trace stream.  All messages will be logged at the info level.

##### OnDemand Configuration
##### Dynamic Configuration
##### Bilge Ping

```
post <base>/bilge-api/bilge-ping/
```
Middleware will write bilge-pong to the response.

#### Redirect Handler

#### Browser Console Handler

