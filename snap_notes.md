# Snap - developed by Canonical, makers of Ubuntu

## What are snap packages?

Snaps are cross-distribution, dependency free, and easy to install applications
packaged with all their dependencies to run on all major Linux distributions. From,
a single build, a snap (application) will run on all supported Linux distributions
on desktop, in the cloud, and IoT. Supported distributions include Ubuntu, Debian,
Fedora, Arch Linux, Manjaro, and CentOS/RHEL.

Snaps are secured--they are confined and sandboxed so that they do not compromise
the entire system. They run under different confinement levels (which is the
degree of isolation from the base system and each other). More notably, every
snap has an interface carefully selected by the snap's creator, based on the
snap's requirements, to provide access to specific system resources outside of their
confinement such as network access, desktop access, and more.

Another important concept in the snap ecosystem is Channels. A channel determines
which release of a snap is installed and tracked for updates and it consists of
and is subdivided by, tracks, risk-levels, and branches.

The main components of a snap package management system are:
* **snapd** - the background service that manages and maintains your snaps on
  a linux system.
* **snap** - both the application pacakge format and the command-line interface
  tool used to install and remove snaps and do many other things in the snap
  ecosystem.
* **snapcraft** - the framework and powerful command-line tool for building snaps.
* **snap store** - a place where developers can share their snaps and Linux users
  search and install them.

Besides, snaps also update automatically. You can configure when and how updates
occur. By default, the `snapd` daemon checks for updates up to 4 times a day: each
update check is called a refresh. You can also manually initate a refresh.

## How to install `snapd` in linux?
`sudo apt install snapd`

After installing `snapd` on your system, enable the `systemd` unit that manages
the main `snap` communication socket, using the systemctl commands as follows:

On ubuntu and its derivatives, this should be triggered automatically by the
package installer.

`sudo systemctl enable --now snapd.socket`

Note that you can't run the `snap` command if the `snapd.socket` is not running.
Run the following commands to check if it is active and is enabled to automatically
start at system boot.

```bash
sudo systemctl is-active snapd.socket
sudo systemctl status snapd.socket
sudo systemctl is-enabled snapd.socket
```

Next, enable classic snap support by creating a symbolic  link between `/var/lib/snapd/snap`
and `/snap` as follows:

`sudo ln -s /var/lib/snapd/snap /snap`

To check the version of `snapd` and snap command-line tool installed on
your system, run the following command.

`snap version`

## How to install snaps packages?

The `snap` command allows you to install, configure, refresh, and remove snaps, and
interact with the larger ecosystem.

Before installing a snap, you can check if it exists in the snap store. For example,
if the app belongs in the  category of "chat servers" or "media players", you can
run these commands to search for it, which will query the store for available packages
in the stable channel.

`snap find "chat servers"`

To show detailed information about a snap, for example, `rocketchat-server`, you
can specify its name or path.

`snap info rocketchat-server`

To install a snap on your system.

`sudo snap install rocketchat-server`

You can opt to install from a different channel: edge, beta, or candidate, for one
reason or the other, using the `--edge`, `--beta`, or `--candidate` options respectively.
Or use the `--channel` option and specify the channel you wish to install from.

`sudo snap install --edge rocketchat-server`

## Manage snaps in linux

To view installed snaps

`snap list`

To list the current revision of a snap being used, specify its name. You
can also list all its available revisions by adding the `--all` option.

`snap list --all mailsping`

### Updating and reverting snaps

You can update a specified snap, or all snaps in the system if none are
specified as follows:

`sudo snap refresh` or `sudo snap refresh mailspring`

After updating an app to a newer version, you can revert to a previously
used version using the `revert` command. Note that the data associated
with the software will also be reverted.

`sudo snap revert mailspring`

Now when you check all revisions of mailspring, the latest revision is
disabled, a previously used revision is now active.

### Disabling / enabling and removing snaps

You can disable a snap if you don't want to use it. When disabled,
a snap's binaries and services will no longer be available, however,
all the data will still be there.

`sudo snap disable mailspring`

If you need to use the snap again, you can enable it back.

`sudo snap enable mailspring`

To completely remove a snap from your system, use the remove
command. By default, all of a snap's revisions are removed.

`sudo snap remove mailspring`

To remove a specific revision, use the `--revision` option as follows.

`sudo snap remove --revision=482 mailspring`

It's key to note that when you remove a snap, its data (such as
internal user, system, and configuration data) is saved by `snapd`
(version 2.39 and higher) as a snapsot, and stored on the system for
31 days. In case you reinstall the snap within 31 days, you can restore
the data.

## How to manage snaps in Linux?

### Run apps from snaps

A snap may provide a single application (or a group of applications) which
you run from the GUI or using commands. By default, all applications associated
to a snap are installed under the `/snap/bin` directory on Debian based distros
and `/var/lib/snapd/snap/bin` for RHEL based distros.

To run an app from the command-line, simply enter its absolute pathname.
`/snap/bin/mailspring`

To only type the app name without typing its full pathname, ensure that the
`/snap/bin` is in your `PATH` environment variable (it should be added by default).

### Creating and using snap aliases

Snap also supports creating aliases for apps. A snap's default (or standard) aliases
have to undergo a public review process before they are enabled, but you can create
aliases for your local system.

You can create an alias for a snap like:

`snap alias mailspring mls`

To run aliases for a snap

`snap aliases mailspring`

To remove an alias for a snap

`snap unalias mls`

### Managing a snap's services

For some snaps, the underlying functionality is exposed through applications
that run as daemons or services, once the snap is installed, they are automatically
started to run continously in the background. Besides, the services are also enabled
to automatically start at system boot. Importantly, a single snap may contain
several applications and services that work together to provide the overall
functionality of that snap.

You can cross-check the services for a snap using the `services` command.

`snap services rocketchat-server`

To stop a service from running, use the `stop` command. Note that this action
is not recommended, as manually stopping a snap's service(s) may cause the
snap to malfunction.

`snap stop rocketchat-server`

To start a servce use the `start` command

`snap start rocketchat-server`

To restart a service after making some custom changes to the snap app, use
the `restart` command. Note that all services for a specified snap will
be restarted by default:

`snap restart rocketchat-server`

To enable a service to automatically start at system boot time

`snap enable rocketchat-server`

To prevent a service from automatically starting at the next system boot,
use the disable command

`snap disable rocketchat-server`

To view the logs for a service, use the `log` command ussing the `-f`
option, which allows you to watch the logs on the screen in real-time.

`snap logs -f rocketchat-server`

**Important**: You can run the above service commands both on individual
snap's services and on all services for a named snap, depending on the
parameter provided.

### Creating and managing a snap's snapshots

`snapd` stores a copy of the user, system, and configuration data for one
or more snaps. You can trigger this manually or set it up to work automatically.
This way, you can backup the state of a snap, revert it to a previous state as
well as restore a fresh `snapd` installation to a previously saved state.

To manually generate a snapshot

`snap save mailspring`

If no snap name is specified, `snapd` will generate snapshots for all installed
snaps (add the `--no-wait` option to run the process in the background to
free up your terminal and allow you to run other commands).

To view the state of all snapshots. You can use the `--id` flag to show
the state of a specific snapshot.

`snap saved`

You can verify the integrity of a snapshot using the `check-snapshot`
command and the snapshot id

`snap check-snapshot 2`

To restore the current user, system, and configuration data with the
corresponding data from a particular snapshot

`snap restore 2`

To delete a snapshot from your system. Data for all snaps are deleted by
default, you can specify a snap to only delete its data.

`snap forget 2 mailspring`

## Notes:

* snaps are containerized apps, bundled with all their dependencies in
  a single package, they are massive (have larger download size) compared
  with just installing the software using `apt` or `yum`.