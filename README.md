# Vagrant configuration files for ServerPilot

This is the setup for having a Vagrant virtual machine working with ServerPilot, sharing files over NFS. The steps are:

1. Install Vagrant and VirtualBox
2. Install `bindfs` for Vagrant:

   ```bash
   $ vagrant plugin install vagrant-bindfs
   ```

3. Download the Vagrantfile from this repository (note that it has been slightly modified from https://github.com/Xety/ServerPilot) and place it into a folder.
4. Follow the steps from https://github.com/Xety/ServerPilot.
5. I had to add the following to the sudoers file using `visudo`, in order to mount the shared folders without having to enter the password each time:

   ```
   Cmnd_Alias VAGRANT_EXPORTS_ADD = /usr/bin/tee -a /etc/exports
   Cmnd_Alias VAGRANT_EXPORTS_COPY = /bin/cp /tmp/exports /etc/exports
   Cmnd_Alias VAGRANT_NFSD_CHECK = /etc/init.d/nfs-kernel-server status
   Cmnd_Alias VAGRANT_NFSD_START = /etc/init.d/nfs-kernel-server start
   Cmnd_Alias VAGRANT_NFSD_APPLY = /usr/sbin/exportfs -ar
   Cmnd_Alias VAGRANT_EXPORTS_REMOVE = /bin/sed -r -e * d -ibak /tmp/exports
   %sudo ALL=(root) NOPASSWD: VAGRANT_EXPORTS_ADD, VAGRANT_NFSD_CHECK, VAGRANT_NFSD_START, VAGRANT_NFSD_APPLY, VAGRANT_EXPORTS_REMOVE, VAGRANT_EXPORTS_COPY
   ```

6. Uncomment the lines `config.vm.synced_folder` and `config.bindfs.bind_folder` from the Vagrantfile, and configure there your paths: the synced folder must be mounted into a temporary path, and the bindfs config must be mounted from that temporary path to the final path.
7. Reload machine:

   ```bash
   $ vagrant reload
   ```

8. You may now start working.
