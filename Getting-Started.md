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
  <DtCredentials type="CREDENTIALS"> <DtLink href="/system/authenticate/credentials" method="POST" name="Submit" rel="action"/> </DtCredentials>
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
## Get DtTaskManager
To create this request, append the href of the `DtTaskManager` link of the `DtVersion` element (see
Retrieve the Available Software Versions) to the base URI. Here is an example of the request URI,
where `<Host>` is the IP address of the tenant or service provider appliance:
```
https://<Host>/dt-rest/v100/task/manager
```
The response contains the following XML:
```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<List type="DtLink">
 <DtLink href="/task/manager" method="GET" name="DtTaskManager" rel="self"/>
 <DtLink href="/task/manager/cancel" method="POST" name="CancelTasks" rel="action"/>
 <DtLink href="/task/manager/tasks" method="POST" name="Tasks" rel="association"/>
 <DtLink href="/task/manager/taskReport" method="POST" name="TaskReport" rel="association"/>
 <DtLink href="/task/manager/childtasks" method="POST" name="GetChildTasks" rel="action"/>
 <DtLink href="/task/manager/filter" method="GET" name="CreateTaskFilter" rel="action"/>
</List>
```
## Retrieve the Task History of a Clone Task
To perform this task, you must first get `DtTaskManager` as described above.
1. Get the tasks report using the `TaskReport` link from the Task Manager response. Here is an
example of the request URI, where `<Host>` is the IP address of the tenant or service provider
appliance:
  ```
  https://<host>/dt-rest/v100/task/manager/taskReport
  ```
  The response contains the following XML:
  ```
  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<DtTaskReport>
 <taskCount>2</taskCount>
 <tasks id="5831">
 <DtLink href="/infrastructure/pool/task/5831" method="GET" name="DtPoolTask" rel="self"/>
 <DtLink href="/infrastructure/pool/task/5831/gettaskhistory" method="GET"
name="GetTaskHistory" rel="action"/>
 <assignmentType>desktop</assignmentType>
 <cancellable>false</cancellable>
 <desktopPoolId>1007</desktopPoolId>
 <hasChildTasks>false</hasChildTasks>
 <percentageComplete>100</percentageComplete>
 <pool href="/infrastructure/pool/desktop/1007" method="GET" name="POD23-AGTB-"
rel="association"/>
 <startDate>2020-08-27T11:39:08.843Z</startDate>
 <status>SUCCESSFUL</status>
 <statusDescription>Request to clone virtual machine 'POD23-AGTB-0378' - Successful</
statusDescription>
 <type>CloneVM</type>
 </tasks>
 <tasks id="5840">
 <DtLink href="/infrastructure/pool/task/5840" method="GET" name="DtPoolTask" rel="self"/>
 <DtLink href="/infrastructure/pool/task/5840/gettaskhistory" method="GET"
name="GetTaskHistory" rel="action"/>
 <assignmentType>desktop</assignmentType>
 <cancellable>false</cancellable>
 <desktopPoolId>1007</desktopPoolId>
 <hasChildTasks>false</hasChildTasks>
 <percentageComplete>100</percentageComplete>
 <pool href="/infrastructure/pool/desktop/1007" method="GET" name="POD23-AGTB-"
rel="association"/>
 <startDate>2020-08-27T11:39:08.843Z</startDate>
 <status>SUCCESSFUL</status>
 <statusDescription>Request to clone virtual machine 'POD23-AGTB-0377' - Successful</
statusDescription>
 <type>CloneVM</type>
 </tasks>
</DtTaskReport>
  ```
2. Get the history of one of the tasks. Here is an example of the request URI, where <Host> is
the IP address of the tenant or service provider appliance:
  ```
  https://<host>/dt-rest/v100/infrastructure/pool/task/5840/gettaskhistory
  ```
  The response contains the following XML:
  ```
  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<List type="DtPoolTaskHistory">
 <DtPoolTaskHistory>
 <dateUpdated>2020-08-27T11:50:18.985Z</dateUpdated>
 <objectId>c3eab94c-c0f7-4651-b8a2-eb1853cc2504</objectId>
 <percentageCompleted>0</percentageCompleted>
 <state>prepared</state>
 <taskId>11916c22-7011-4b34-8461-b524d43df074</taskId>
 </DtPoolTaskHistory>
 <DtPoolTaskHistory>
 <dateUpdated>2020-08-27T11:50:28.379Z</dateUpdated>
 <objectId>c3eab94c-c0f7-4651-b8a2-eb1853cc2504</objectId>
 <percentageCompleted>0</percentageCompleted>
 <state>queued</state>
 <taskId>11916c22-7011-4b34-8461-b524d43df074</taskId>
 </DtPoolTaskHistory>
 <DtPoolTaskHistory>
 <dateUpdated>2020-08-27T11:50:28.548Z</dateUpdated>
 <objectId>c3eab94c-c0f7-4651-b8a2-eb1853cc2504</objectId>
 <percentageCompleted>0</percentageCompleted>
 <state>running</state>
 <taskId>11916c22-7011-4b34-8461-b524d43df074</taskId>
 </DtPoolTaskHistory>
 <DtPoolTaskHistory>
 <dateUpdated>2020-08-27T11:50:40.950Z</dateUpdated>
 <description>Request to clone virtual machine 'POD23-AGTB-0377' - Successful</description>
 <objectId>c3eab94c-c0f7-4651-b8a2-eb1853cc2504</objectId>
 <percentageCompleted>100</percentageCompleted>
 <state>success</state>
 <taskId>11916c22-7011-4b34-8461-b524d43df074</taskId>
 </DtPoolTaskHistory>
</List>
  ```
## Get DtMappingManager
To retrieve `DtMappingManager`, use the path information specified by the `DtMappingManager` link of
the `DtVersion` element (this request must include the Authorization header):
```
<DtLink href="/mapping/manager" method="GET" name="DtMappingManager" rel="top"/>
```
The href attribute indicates the URI to append to the base URI. Using this information, you can
construct the following HTTP request:
```
https://Host/dt-rest/v100/mapping/manager
```
The response specifies the links you can traverse to obtain mapping of users and virtual
machines to assignments. The following XML fragment shows two of the returned links:
```
<DtLink href="/mapping/manager/usermappingreport?
sortcriteria={sortcriteria}&amp;sortorder={sortorder}&amp;" method="POST" name="UserMappingReport"
rel="action"/>
 <DtLink href="/mapping/manager/desktopmappingreport?
sortcriteria={sortcriteria}&amp;sortorder={sortorder}&amp;" method="POST" name="DesktopMappingReport"
rel="action"/>
```
## Retrieve the Report of Mapped Users to Assignments
To retrieve a report of mapped users to all assignments in the tenant, use the `UserMappingReport`
method of the Mapping Manager:
```
<DtLink href="/mapping/manager/usermappingreport?
sortcriteria={sortcriteria}&amp;sortorder={sortorder}&amp;" method="POST" name="UserMappingReport"
rel="action"/>
```
The href attribute indicates the URI to append to the base URI. Using this information, you can
construct the following HTTP request (must include the Authorization header):
```
https://Host/dt-rest/v100/mapping/manager/usermappingreport?sortcriteria=Id&sortorder=Ascending
```
The response contains one `<UserMapping>` element for each user with all its properties, including
the assignment to which the user is mapped. For example:
```
<DtUserMappingReport>
 <userMappingReportCount>3</userMappingReportCount>
 <userMappingReportTotalCount>3</userMappingReportTotalCount>
 <UserMapping>
 <assignmentName>bat-vdi</assignmentName>
 <desktopModel>selectable</desktopModel>
 <desktopName>Small</desktopName>
 <mapType>static</mapType>
 <mappingSource>direct</mappingSource>
 <poolType>desktop</poolType>
 <userDomain>AUTO-TENANTD</userDomain>
 <userGuid>eb5965f04136634dbd4b75784d3d44e3</userGuid>
 <userName>portaluser1</userName>
 <vmName>D-bat-vdi0000</vmName>
 </UserMapping>
 <UserMapping>
 <assignmentName>bat-dsktp</assignmentName>
 <desktopModel>selectable</desktopModel>
 <desktopName>Hosted App Server</desktopName>
 <farmName>bat-dsktp</farmName>
 <groupName>portalusers</groupName>
 <mapType>session</mapType>
 <mappingSource>group</mappingSource>
 <poolType>session</poolType>
 <userDomain>AUTO-TENANTD</userDomain>
 <userGuid>eb5965f04136634dbd4b75784d3d44e3</userGuid>
 <userName>portaluser1</userName>
 <vmName></vmName>
 </UserMapping>
 <UserMapping>
 <assignmentName>bat-apps</assignmentName>
 <desktopModel>selectable</desktopModel>
 <desktopName>Hosted App Server</desktopName>
 <farmName>bat-apps</farmName>
 <groupName>portalusers</groupName>
 <mapType>appsession</mapType>
 <mappingSource>group</mappingSource>
 <poolType>appsession</poolType>
 <userDomain>AUTO-TENANTD</userDomain>
 <userGuid>eb5965f04136634dbd4b75784d3d44e3</userGuid>
 <userName>portaluser1</userName>
 <vmName></vmName>
 </UserMapping>
</DtUserMappingReport>
```
