# Understanding the API
The REST API provides access to features and functionality (resources) using REST-based web
services. Exchanges between the caller and the platform take place using HTTP and HTTPS
requests and the platform returns XML to the caller in the HTTP response.
## Resources
The following are the top-level entry points for the REST API.
>**Warning** `DtSecurityManager` is not included in the following table because it is strongly
recommended that you do not use it. Since functions accessed through this entry point cannot
be fully controlled with REST APIs, avoid using any of the resources under `DtSecurityManager`.

| Level | Entry Point | Description |
| ----------- | ----------- | ----------- |
| 1 | `DtPlatform` | <ul><li>Top-level entry point for the REST API</li></ul> |
| 2 | `DtVersion` | <ul><li>Under `DtPlatform`</li><li>Provides version-related information about the REST APIs</li></ul> |
| 3 | <code>DtInfrastructureManager</code><br><code>DtInstallManager</code><br><code>DtNotificationManager</code><br><code>DtPoolManager</code><br><code>DtQuotaManager</code><br><code>DtReportingManager</code><br><code>DtSessionManager</code><br><code>DtSettingsManager</code><br><code>DtSystemManager</code><br><code>DtTaskManager</code> | <ul><li>Under <code>DtPlatform</code></li><li>Each of these is a top-level entry point for a category of REST API resources. For example, under <code>DtInfrastructureManager</code>, there are <code>DtDataCenter</code>, <code>DtNetwork</code>, and so on.</li></ul> |
## Scope
The REST APIs have a common object model across the management (service provider and
tenant) appliances. Some resources are available to both service providers and tenants, some are
available only to service providers, and some available only to tenants. Among the resources that
are available to both service providers and tenants, there is a scope associated with individual
links in the resource instances that restricts the availability of some of the links to service
providers or tenants. As a result, the same resource contains different links based on the context
of its retrieval.
## Properties and Links
Resources contain properties and links:
* `name` – The unique name of the link. The name typically describes the purpose or target of the
attribute.
* `href` – The URI relative to the server, webapp, and version, for example `/infrastructure/pool/
1000`
* `method` – The http method used with the href in the REST API invocation: `GET`, `POST`, `PUT`, or
`DELETE`.
* `rel` – Describes the relationship between the link and the resource that contains the link. The
`re`l attribute is for informational purposes and is not required when coding to the API. The
following table defines the link relationships.
## Link Attributes
A link has the following attributes:
* `name` – The unique name of the link. The name typically describes the purpose or target of the
attribute.
* `href` – The URI relative to the server, webapp, and version, for example `/infrastructure/pool/
1000`
* `method` – The http method used with the href in the REST API invocation: `GET`, `POST`, `PUT`, or
`DELETE`.
* `rel` – Describes the relationship between the link and the resource that contains the link. The
rel attribute is for informational purposes and is not required when coding to the API. The
following table defines the link relationships. 

| Relationship | Description |
| ----------- | ----------- |
| `ACTION` | Indicates that the URI performs an action with the resource |
| `ASSOCIATION` | Indicates that the URI retrieves an associated resource |
| `MAP` | Indicates that the URI retrieves a map between resources (one-one, one-many, many-many) | 
| `SELF` | Indicates that the URI is the URI of the container resource. A REST response always contains a link to itself. A `GET` on this URI returns the same URI. |
| `TOP` | Indicates that the URI is a top-level root resource |
## HTTP Requests
The HTTP request you use to retrieve a Horizon DaaS Platform resource is constructed as
follows:
```
https://<host>/dt-rest/<version>/<URI>
```
* host is the IP address of the administration console or Service Center.
* version is the version of the REST API. For information on retrieving available versions, see
Chapter 2 Getting Started.
* URI is the path to the resource, constructed by traversing the object model and extracting
path information from the href attribute of a link. For example:
  ```
  /infrastructure/manager/mapping/users/default?sep={sep}&users={users}&
  ```
* The first component of the path is always the functional area, such as system or
infrastructure.
* The /manager component of the path is used only for top-level resources and links served by
that resource.
* Curly braces are used to represent request parameter values which you must supply in the
actual HTTP request. The href attribute uses the following syntax to represent request
parameters:
  ```
  ?p1={p1}&p2={p2}
  ```
  For the preceding example, the actual values for the two request parameters (sep and users)
might look as follows (where sep specifies the character you are using to delimit the user IDs
in the request):
  ```
  ?sep=,&users=1234,5678&
  ```
* URL encoding is used to replace characters outside the ASCII set with a percent sign
character (%) followed by two hexadecimal digits. For example, the question mark character
(?) is replaced with %3F and the ampersand character (&) is replaced with %26.<br>
  Here is the complete HTTP request:
  ```
  https://Host/dt-rest/v100/infrastructure/manager/mapping/users/default?sep=,&users=1234,5678&
  ```
