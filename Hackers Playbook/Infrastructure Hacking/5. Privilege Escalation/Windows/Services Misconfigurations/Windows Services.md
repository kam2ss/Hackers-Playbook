Windows services are managed by the **Service Control Manager (SCM)** which controls service states, checks statuses, and allow configuration.

Each service has an associated executable run by the SCM when the service starts. These ***executables must have a special functions to communicate with the SCM***, so not all executables can run as services. 

**Check the `apphostsvc` service configuration:**
```
sc qc apphostsvc
```
- associated executable is specified through the **BINARY_PATH_NAME** parameter
- account used to run the service is shown on the **SERVICE_START_NAME** parameter

***Services have a Discretionary Access Control List (DACL)***, which indicates who has permission to start, stop, pause, query status, query configuration, or reconfigure the service.

All of the ***services configurations are stored on the registry*** under: `HKLM\SYSTEM\CurrentControlSet\Services\`. If a DACL is configured for the service, it will be stored in a subkey called `security`.
- `ImagePath:` show the associated executable
- `ObjectName:` account used to start the service

