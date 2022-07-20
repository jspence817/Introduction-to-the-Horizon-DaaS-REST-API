# Getting Started
This section gives you the basic information to get started using the REST API.
The instructions in this section describe how to:
* Retrieve the available software versions using the published URL /system/platform.
* Log in and save the HTTP response headers.
* Get the DtTaskManager.
* Retrieve the task history of a clone task.
* Get DtMappingManager.
* Retrieve the report of mapped users to assignments.
Before using the samples in this chapter to begin coding to the API, you must have the tenant or
service provider IP address.
## Retrieve the Available Software Versions
To retrieve the available software versions, submit the following HTTP request, where <Host> is
the IP address of the tenant or service provider appliance.<br>
```
https://<Host>/dt-rest/system/platform
```
The response includes a single DtPlatform XML element that contains one DtVersion element for
each available software version. The latest version is the DtVersion element for which the
attributelatest=”true”. For example:
```
<DtVersion latest="true" id="v100">
  <DtLink href="/system" method="GET" name="DtSystemManager" rel="top"/>
  <DtLink href="/maintenance/manager" method="GET" name="DtMaintenanceManager" rel="top"/>
  <DtLink href="/install/manager" method="GET" name="DtInstallManager" rel="top"/>
  <DtLink href="/install/orchestrationengine" method="GET" name="DtOrchestrationEngine" rel="top"/>
  <DtLink href="/security/manager" method="GET" name="DtSecurityManager" rel="top"/>
  <DtLink href="/setting/manager" method="GET" name="DtSettingsManager" rel="top"/>
  <DtLink href="/infrastructure/manager" method="GET" name="DtInfrastructureManager" rel="top"/>
  <DtLink href="/session/manager" method="GET" name="DtSessionManager" rel="top"/>
  <DtLink href="/quota/manager" method="GET" name="DtQuotaManager" rel="top"/>
  <DtLink href="/mapping/manager" method="GET" name="DtMappingManager" rel="top"/>
  <DtLink href="/reservation/manager" method="GET" name="DtReservationStatusManager" rel="top"/>
  <DtLink href="/pool/manager" method="GET" name="DtPoolManager" rel="top"/>
  <DtLink href="/task/manager" method="GET" name="DtTaskManager" rel="top"/>
  <DtLink href="/reporting/manager" method="GET" name="DtReportingManager" rel="top"/>
  <DtLink href="/example/top" method="GET" name="DtTopLevelManagerImpl" rel="top"/>
  <DtLink href="/notification/manager" method="GET" name="DtNotificationManager" rel="top"/>
  <domainRegistrationURI>/v100/system/register/domain</domainRegistrationURI>
  <DtCredentials type="CREDENTIALS">
    <DtLink href="/system/authenticate/credentials" method="POST" name="Submit" rel="action"/>
  </DtCredentials>
</DtVersion>
```
The DtVersion element also defines the other top-level links you use to traverse the REST API,
including DtSystemManager and DtInfrastructureManager. 
## Log In and Save the HTTP Response Header Authorization
The API provides secure access using domain authentication or two-factor authentication if
configured.
1. To form the URL for your login, copy and paste the href from the authentication step
returned by DtVersion.
  DtVersion returns an authentication step, which varies depending on what authentication is
configured for the organization. For example, if only Active Directory authentication is
configured, it is DtCredentials, shown here (the href value is /system/authenticate/
credentials):
  ```
  DtCredentials type="CREDENTIALS" <DtLink href="/system/authenticate/credentials" method="POST" name="Submit" rel="action"/> </DtCredentials>
  ```
2. To form the message to send for login, look in the documentation for the appropriate
resource and include required properties, as in the following example.
  ```
  <DtCredentials type="CREDENTIALS">
<username>nnnnnnnnn</username>
<password>xxxxxxxx</password>
<domain>DEMO-TENANT</domain>
</DtCredentials>
  ```
3. Send message as `POST`.
4. In the HTTP response header, note the following two values:
    * Authorization
    * x-dt-csrf-header
  
Note the following:
  * You must include the Authorization key-value pair in the HTTP request header for every
subsequent HTTP request you submit after login.
  * If a request is a type other than `GET`, you must also include the x-dt-csrf-header key-value
pair.
