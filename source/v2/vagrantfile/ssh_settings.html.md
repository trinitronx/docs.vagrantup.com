---
sidebar_current: "vagrantfile-ssh"
---

# SSH Settings

**Config namespace: `config.ssh`**

The settings within `config.ssh` relate to configuring how Vagrant
will access your machine over SSH. As with most Vagrant settings, the
defaults are typically fine, but you can fine tune whatever you'd like.

## Available Settings

`config.ssh.default.host` - This sets the default host that Vagrant will
use for SSH. This is not set by default because the providers usually
detect the proper host. Provider-detected hosts will override this. To
force a certain host, use `config.ssh.host`.

<hr>

`config.ssh.default.port` - This sets the default port that Vagrant will
use to connect to the machine via SSH. This is not set by default because
the providers usually detect the proper port. Provider-detected ports will
override this. To force a certain port, use `config.ssh.port`.

<hr>

`config.ssh.default.private_key_path` - This sets the default private
key to use for authenticating with SSH. By default this is set to the
insecure private key that ships with Vagrant, since that is what most
public boxes use. A provider-detected private key will override this setting.
To force a certain private key, use `config.ssh.private_key_path`. The
private key format should be an OpenSSH private key format (as opposed
to PuTTY keys and such).

<hr>

`config.ssh.default.username` - This sets the default username that
Vagrant will SSH as, if no other username can be detected. This is set
to "vagrant" by default, since that is what most public boxes use.
Alternate providers may detect alternate usernames though, in which
case this will be overriden. To force a certain username, use `config.ssh.username`.

<hr>

`config.ssh.host` - The same as `config.ssh.default.host`, except this
will override any detected host and will always be used.

<hr>

`config.ssh.port` - The same as `config.ssh.default.port`, except this
will override any detected port and will always be used.

<hr>

`config.ssh.private_key_path` - The same as `config.ssh.default.private_key_path`,
except this will override any detected private key and will always be used.

<hr>

`config.ssh.username` - The same as `config.ssh.default.username`, except
this will override any detected username and will always be used.

<hr>

`config.ssh.guest_port` - The port on the guest that SSH is running on. This
is used by some providers to detect forwarded ports for SSH. For example, if
this is set to 22 (the default), and Vagrant detects a forwarded port to
port 22 on the guest from port 4567 on the host, Vagrant will attempt
to use port 4567 to talk to the guest if there is no other option.

<hr>

`config.ssh.keep_alive` - If true, a "keep-alive" packet will be sent
every 5 seconds on the SSH connection. This avoids the connection
closing on long-running tasks. This defaults to true.

<hr>

`config.ssh.max_tries` - Maximum attempts to SSH while waiting for the
machine to boot. Default is 100.

<hr>

`config.ssh.timeout` - Maximum time to wait while attempting to make
a single connection via SSH before timing out. Default is 30 seconds.

<hr>

`config.ssh.forward_agent` - If `true`, agent forwarding over SSH
connections is enabled. Defaults to false.

<hr>

`config.ssh.forward_x11` - If `true`, X11 forwarding over SSH connections
is enabled. Defaults to false.

<hr>

`config.ssh.shell` - The shell to use when executing SSH commands from
Vagrant. By default this is `bash -l`. Note that this has no effect on
the shell you get when you run `vagrant ssh`. This configuration option
only affects the shell to use when executing commands internally in Vagrant.
