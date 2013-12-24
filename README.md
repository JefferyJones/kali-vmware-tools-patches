vmware-tools-patches for Kali Linux kernel 3.12
===============================================

These bash scripts allow you to easily apply multiple patches to a `VMwareTools-*.tar.gz` file.

It has been tested with the following files:

* VMwareTools-9.2.3-1031360.tar.gz (VMWare Workstation 9.0.2)
* VMwareTools-9.6.0-1294478.tar.gz (VMWare Workstation 10.0.0)
* VMwareTools-9.6.1-1378637.tar.gz (VMWare Workstation 10.0.1)

To run, execute these commands inside the Kali Linux VMware Guest:

1. Prepare Kali Linux for vmware-tools installation:
<pre>
$ apt-get install linux-headers-$(uname -r)
$ sed -i '/cups\ disabled/d' /usr/sbin/update-rc.d 
$ echo cups enabled >> /usr/sbin/update-rc.d 
$ echo vmware-tools-thinprint enabled >> /usr/sbin/update-rc.d
</pre>

2. Checkout the vmware-tools-patches repository:
<pre>
$ git clone https://github.com/offensive-security/vmware-tools-patches
</pre>

3. Copy a `VMwareTools-*.tar.gz` into the `vmware-tools-patches` folder:
<pre>
$ cp VMwareTools-*.tar.gz vmware-tools-patches/
</pre>

4. Apply the patches, and then build the vmware-tools:
<pre>
$ cd vmware-tools-patches
$ export VMWARE_TOOLS_PATCHES_DEBUG=1
$ ./untar-all-and-patch.sh
$ cd vmware-tools-distrib
$ sed -i "s/'RUN_CONFIGURATOR', 'yesno', 'yes');/, 'RUN_CONFIGURATOR', 'yesno', 'no');/" vmware-install.pl 
$ ./vmware-install.pl -d
$ cd ..
# vmware-tools patch taken from http://blog.spiderlabs.com/2013/09/installing-vmware-tools-on-kali-linux-and-some-debugging-basics.html (thanks guys!)
$ patch -p0 < patches/vmware-tools.patch 
$ vmware-config-tools.pl -d
</pre>
