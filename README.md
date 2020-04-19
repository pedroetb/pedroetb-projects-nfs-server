# nfs-server

## AppArmor

If your Docker host has [AppArmor](https://wiki.ubuntu.com/AppArmor) activated, you'll need to perform additional steps to allow the container to start an NFS server.

1. Ensure you have the `apparmor-utils` installed package installed on the Docker host. e.g. for Debian or Ubuntu:

       $ sudo apt-get install apparmor-utils

1. Create a file on the Docker host with the following contents:

       #include <tunables/global>
       profile nfs-server flags=(attach_disconnected,mediate_deleted) {
         #include <abstractions/lxc/container-base>
         mount fstype=nfs*,
         mount fstype=rpc_pipefs,
       }

1. Load this profile into the kernel with [`apparmor_parser`](http://manpages.ubuntu.com/manpages/xenial/man8/apparmor_parser.8.html):

       $ sudo apparmor_parser -r -W /path/to/file/from/previous/step
