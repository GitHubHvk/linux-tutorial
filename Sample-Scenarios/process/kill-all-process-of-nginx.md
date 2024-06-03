# Subject

we want to find all process of `nginx`, then kill them all in one command.



# list processes

to see all running processes of `nginx` execute the following command:

```powershell
ps aux | grep nginx
```



# kill all the processes

one simple way is to run the `kill` command for each of `PID` in the output of above command like `kill -9 95685`.

but there is a simpler solution, and that is to pass the list of PIDs to `kill` command. to do so execute the following command:

```powershell
pidof nginx | xargs kill -9 
```



description of each parts of the above command would be as follow:

- `pidof nginx`
  - this command output all PIDs related to `nginx`.
- |
  - will pass the output of previous command the next command.
- `xargs`
  - prepare incoming stream of previous command as arguments of the next command
- kill -9
  - based on inputs from `xargs`, kill the processes by ID.   