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
| 1 | <code>DtPlatform</code> | <ul><li>Top-level entry point for the REST API</li></ul> |
| 2 | <code>DtVersion</code> | <ul><li>Under <code>DtPlatform</code><br>Provides version-related information about the REST APIs</li></ul> |
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
<ul><li>A property stores a value. For example, the <code>DtUser</code> resource has a <code>loginName</code> property that indicates the name used by a user to log in.</li><li>A link specifies a URI of a related resource or action. For example, the <code>DtUser</code> resource has a <code>desktopPatterns</code> link that retrieves the desktop patterns assigned to a user.</li></ul>
## Link Attributes

A link has the following attributes:
<ul><li><code>name</code> – The unique name of the link. The name typically describes the purpose or target of the
attribute.</li>
<li><code>href</code> – The URI relative to the server, webapp, and version, for example /infrastructure/pool/
1000</li>
<li><code>method</code> – The http method used with the href in the REST API invocation: <code>GET</code>, <code>POST</code>, <code>PUT</code>, or <code>DELETE</code></li>
<li><code>rel</code> – Describes the relationship between the link and the resource that contains the link. The
rel attribute is for informational purposes and is not required when coding to the API. The
following table defines the link relationships.</li>  
