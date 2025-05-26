#### Task Scheduler
The most common way to schedule tasks is using the built-in **Windows task scheduler**. The task scheduler allows for granular control over a task. From the command line, you can use `schtasks` to interact with the task scheduler.

#### Create a task that runs a reverse shell every single minute.
```
schtasks /create /sc minute
```