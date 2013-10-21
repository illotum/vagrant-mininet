# vagrant-mininet

This Vagrantfile tries to be an all-inclusive script to provide you
OpenFlow playground in the VM.

Included tools are:

- Mininet
- POX
- Wireshark dissector
- Trema
- Flowvisor

## Usage

You have to have VirtualBox and Vagrant installed. Next, download the
Vagrantfile and run `vagrant up` from the folder it is located.

After the script finishes its job, `vagrant ssh` should provide you the
VM terminal. Windows users may want to install `msysgit` distribution for a
seamless ssh experience.
