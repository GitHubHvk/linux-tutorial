# `systemd`

in many Linux distributions there a service named `systemd` that is used to manage other services and system settings.

this service organizes tasks into components called `unites` , and groups of units into `targets`.

this service can start `units` and `targets` at boot time, or when requested by user or another `systemd` target.

each service has a config file that is used by `systemd` and tells it how to manage the service, for example when to start it (before or after which services), dependencies to other services, command needed to be executed in start, stop, and reload, and some other info. mainly these files are located in path `lib/systemd/system`.



***



# `systemctl`

this is a command to interact with processes controlled by `systemd`. it has following mainly used options:

- start

  - this option is used to start a service. the syntax is `systemctl start [service name]`.

  - ```powershell
    systemctl start nginx
    ```

- stop

  - this option is used to stop a service. the syntax is `systemctl stop [service name]`.

  - ```powershell
    systemctl stop nginx
    ```

- enable

  - if you want a service to be started after system booted, you can use this option. the syntax is `systemctl enable [service name]`.

  - ```powershell
    systemctl enable nginx
    ```

- disable

  - if you want a service to not start after system booted, you can use this option. the syntax is `systemctl disable [service name]`.

  - ```powershell
    systemctl disable nginx
    ```

- mask

  - when a service is masked, it can not be started. the syntax is `systemctl mask [service name]`.

- unmask

  - when a service is masked, it can not be started, to make it possible to start it again we need to unmask it. the syntax is `systemctl unmask [service name]`.

- status

  - will show you the status of the service. also show some other info including the path of the file that system configuration is loaded from. 
  - the status of a service could be one of the following:
    - active
    - inactive
    - failed



# edit config

`systemctl` provides built-in mechanisms for editing and modifying unit files if you need to make adjustments.

The `edit` command, by default, will open a unit file snippet for the unit in question:

```powershell
systemctl edit nginx.service
```



This will be a blank file that can be used to override or add directives to the unit definition. A directory will be created within the `/etc/systemd/system` directory which contains the name of the unit with `.d` appended. For instance, for the `nginx.service`, a directory called `nginx.service.d` will be created.

Within this directory, a snippet will be created called `override.conf`. When the unit is loaded, `systemd` will, in memory, merge the override snippet with the full unit file. The snippet’s directives will take precedence over those found in the original unit file.



If you wish to edit the full unit file instead of creating a snippet, you can pass the `--full` flag:

```powershell
systemctl edit --full nginx.service
```



This will load the current unit file into the editor, where it can be modified. When the editor exits, the changed file will be written to `/etc/systemd/system`, which will take precedence over the system’s unit definition (usually found somewhere in `/lib/systemd/system`).



# remove modified config

To remove any additions you have made, either delete the unit’s `.d` configuration directory or the modified service file from `/etc/systemd/system`. For instance, to remove a snippet, we could type:

```powershell
rm -r /etc/systemd/system/nginx.service.d
```



To remove a full modified unit file, we would type:

```powershell
rm /etc/systemd/system/nginx.service
```



After deleting the file or directory, you should reload the `systemd` process so that it no longer attempts to reference these files and reverts back to using the system copies. You can do this by typing:

```powershell
systemctl daemon-reload
```



# service log

to see logs of service we can use command `journalctl`. for example if you run command `journalctl -xefu nginx`, you will see interactive ((f)) logs related to `nginx` unit ((u)).