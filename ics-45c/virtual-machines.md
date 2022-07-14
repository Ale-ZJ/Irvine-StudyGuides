---
description: ics45c-2021spring
---

# Virtual Machines

We are using a Virtual Machine for the class. VMware is a **hypervisor** used to arbitrate virtualization between the host and the guest.

#### Useful commands to navigate a Linux os:

* `passwd` to change password
* `ls` list the files residing in the current working directory
* `cd` move between directories
* `.` current directory
* `..` previous directory
* `ip addr` shows network info
* `df` shows the&#x20;
* `sudo shutdown -h now` safe way to shutting down the VM
* `sudo reboot` to reboot VM
* `sudo netplan apply` to restart the network interfaces

#### These are useful commands for writing projects in the VM:

* `ics45c` lists all commands available in the ics45c VM
* `ics45c start PROJ_NAME TEMPLATE` creates a new project in `~/projects`&#x20;
* `ics45c version` displays current version with timestamp&#x20;
* `ics45c refresh` connects to course website and downloads the lastest version of `environment`
* `ics45c restore` when you accidentally delete or corrupt your environment&#x20;
* `./build`&#x20;
  * `app`
  * `exp`
  * `gtest`
* `./run app`
* `./run app >output.txt`
* `./run app <inputs/sample.in`
* `./run --memcheck gtest`&#x20;
* `more output.txt` show one page of info

#### Text editors you can use:

* `nano FILE_NAME`
* `vim FILE_NAME`
* `code FILE_NAME`
* `emacs FILE_NAME`

\*Note: to run a program in the VM, there shsould be no warnings nor errors from the compiler.

