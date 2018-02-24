Sample init scripts and service configuration for exsolutiond
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/exsolutiond.service:    systemd service unit configuration
    contrib/init/exsolutiond.openrc:     OpenRC compatible SysV style init script
    contrib/init/exsolutiond.openrcconf: OpenRC conf.d file
    contrib/init/exsolutiond.conf:       Upstart service configuration file
    contrib/init/exsolutiond.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "exsolution" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, exsolutiond requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, exsolutiond will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that exsolutiond and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If exsolutiond is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/exsolution/exsolution.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/exsolution.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/exsolutiond
Configuration file:  /etc/exsolution/exsolution.conf
Data directory:      /var/lib/exsolutiond
PID file:            /var/run/exsolutiond/exsolutiond.pid (OpenRC and Upstart)
                     /var/lib/exsolutiond/exsolutiond.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the exsolution user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
exsolution user and group.  Access to exsolution-cli and other exsolutiond rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start exsolutiond" and to enable for system startup run
"systemctl enable exsolutiond"

4b) OpenRC

Rename exsolutiond.openrc to exsolutiond and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/exsolutiond start" and configure it to run on startup with
"rc-update add exsolutiond"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop exsolutiond.conf in /etc/init.  Test by running "service exsolutiond start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy exsolutiond.init to /etc/init.d/exsolutiond. Test by running "service exsolutiond start".

Using this script, you can adjust the path and flags to the exsolutiond program by
setting the EXTD and FLAGS environment variables in the file
/etc/sysconfig/exsolutiond. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
