# USN Infrastructure Exam

## Security

### Identity, Authentication and Authorization
- Local Account
- Windows Auth
- SAML 2.0
- Google Auth
- Azure AD Auth

### Orchestrator

- Must be accessible by `Robots` and `Users`
- Must be secured using `HTTPS` and latest ciphers
- Not Recom to expose to `public`
- Encrypt each tenant using `azure key vault`
- Sensitive info should not be stored in `Queues`
- Use `Azure ExpressRoute` VPN when accssing orch


```
messages are sent from the Robot to Orchestrator with a frequency of 1 message per second
within 60 seconds, the Robot sends:
15 message logs
2 heartbeats
6 get asset requests
6 add queue item requests
6 get queue item requests
```

## Logging

By default logs are forwaded and stored in MSSQL Database

### Elasticsearch

- Free and open source analytics engine based of Apache Lucene
- NoSQL DB
- data is stored in an unstructured way

### NLog

- Orchestrator uses the NLOG lib to collect messages from robots and redirects them to one or more targets
- Supports custom targets

### Insights

- Can only be installed on windows
- Can be installed on Linux but requires additional requirements
- Needs separate MSSQL DB
- within Insights DB, tables are created for robots logs, processes, jobs, queues, and licensing
- Insights must be manually enabled for each tenant in Orch
  - due to perf and server cost implications
- Oout of the box dashboards are created per tenant

## Database

- Encryption
  - Data at rest (TDE)
  - Data in flight (TLS)
- Permissions'
  - Use Mixed Mode auth and use windows service account
  - Same account running app pool on your orch
  - do not provide the account sysad priv
  - Use least permissions: db_datareader, db_datawriter, db_ddladmin and Execute permmions on DBO
- Certificates
  - Cert must be installed in the local maching personal store and configured for use in SQL server manager
- Maintenance Activies
  - Follow ola hallengren
  - DatabaseIntegrityCheck
  - DatabaseBackup
  - IndexOptimize
- Large DB tables 
  - Logs
  - Jobs
  - AuditLogs
  - AuditLogEntities
  - TenantNotifications
  - UserNotifications
  - QueueItems
  - QueueItemEvents
  - QueueItemComments
  - RobotLicenseLogs


## Robots

- Unattended 
  - Non Prod
  - Testing
    - can execute test cases and RPA processses intended fo r non prod env
- Security
  - Enforce Signed Execution
  - Disable Secure XAMLS
  - Disable Telemetry
-

- Communication Channels
  - SignalR
  - WebSockets
  - Server-Sent Events
  - Long Polling
  -  1