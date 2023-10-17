---
title: Application Sandboxing
---

One of the motivations for leveraging systemd in `applaunchd` was to allow the use of its
advanced features such as [sandboxing](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Sandboxing),
[system call filtering](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#System%20Call%20Filtering),
[process limits](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Process%20Properties), and
Linux CGroup configuration via [slices](https://www.freedesktop.org/software/systemd/man/systemd.slice.html).
The scope of potential systemd configurations involving these options is broad, and so far AGL has focused on providing
some simple examples of basic sandboxing integrated with the application templates discussed above.

## Network Access Restriction

The systemd [`PrivateNetwork`](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#PrivateNetwork=)
service configuration option can be used to disable network access for the service started by a unit file.
The `applaunchd` packaging installs a systemd drop-in configuration file named `no-network.conf` in the
directory `/usr/lib/systemd/system/sandboxing` that may be used to disable network access with `PrivateNetwork`.
To use it, `no-network.conf` should be symlinked in an appropriate unit file drop-in override directory per
systemd naming expectations (see systemd unit file
[man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description)).
The following is a `BitBake` recipe snippet showing installation of the drop-in for an application named `foo`
that is using the generic application template:

```text
...

inherit agl-app

AGL_APP_NAME = "Foo"

do_install() {
    # Enable systemd sandboxing override as a demonstration
    install -d ${D}${systemd_system_unitdir}/agl-app@${AGL_APP_ID}.service.d/
    ln -sf ${systemd_system_unitdir}/sandboxing/no-network.conf ${D}${systemd_system_unitdir}/agl-app@${AGL_APP_ID}.service.d/
}

FILES:${PN} = " \
    ${sysconfdir}/systemd/system/agl-app@${AGL_APP_ID}.service.d \
"

...
```

This results in a `/usr/lib/systemd/system/agl-app@foo.service.d/no-network.conf` symlink being created
in the `foo` packaging, disabling network access when `foo` is started by `applaunchd` or directly via
`systemctl` on the command line.

## Private Temporary Directories

The systemd [`PrivateTmp`](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#PrivateTmp=)
service configuration option can be used to prevent access to the host `/tmp` and `/var/tmp` directories
for the service started by a unit file.  The service will instead have private temporary mounts of these
directories in a new mount namespace.  The `applaunchd` packaging installs a systemd drop-in configuration
file named `private-tmp.conf` in the directory `/usr/lib/systemd/system/sandboxing` that may be used to
create private temporary directory mounts with `PrivateTmp`. To use it, `private-tmp.conf` should be
symlinked in an appropriate unit file drop-in override directory per systemd naming expectations (see
systemd unit file [man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description)).
See above for an example `BitBake` recipe snippet showing installation of a drop-in file when using the
generic application template.

## Other Sandboxing Options

In addition to the above, some other notable systemd sandbox options worth further consideration for
applications include:

- [PrivateDevices](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#PrivateDevices=)  
  This option could be used to remove access to device files in /dev for applications that do not
  require them, but it is likely that `DeviceAllow` may also need to be used to enable functionality in
  some applications.
- [ProtectSystem](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ProtectSystem=)  
  The default and `full` modes of this option likely can be enabled with little impact to most
  applications, as they should not need write access to the directories that are made read-only.
  Using the `strict` option would need investigation into interactions with usage of directory
  hierarchies like `/run`.
- [ProtectHome](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ProtectHome=)  
  This option seems worthwhile if the system ends up with more than one active user for running
  applications, but interactions with default application caching and configuration locations
  needs to be investigated.  It is likely that enforcing use of locations outside of `/home`
  for these would need to be settled upon and documented for application developers.  For
  example using the [XDG directory scheme](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)
  similar to the previous AGL application framework.
- [ReadWritePaths, etc.](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ReadWritePaths=)  
   These options provide a more fine-grained approach to some of the above ones, and could be
   used to enable something like an application only seeing its own files and the contents of
   a specific set of directories like `/etc` and `/usr/lib`.  Implementing such would likely
   require settling on a custom application installation hierarchy as was done in the previous
   AGL application framework.

The above list is not exhaustive, and does not include some other likely possibilities involving
system call filtering and resource limits.  Future AGL releases may expand the examples provided
with `applaunchd` along these lines.
