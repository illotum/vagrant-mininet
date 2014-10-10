# -*- mode: ruby -*-
# vi: set ft=ruby :

$init = <<SCRIPT
if [ ! -f ~/runonce ]
then
  ## Init
  #locale-gen en_CA.UTF-8 # Feel free to change it to your locale
  aptitude update
  aptitude install -yq git-core libpcap-dev libsqlite3-dev gcc make \
    pkg-config autotools-dev libltdl-dev libltdl7 m4 cmake cgroup-lite wireshark \
    libxslt1-dev msgpack-python python-setuptools python-nose python-pip python-dev

  ## Mininet, POX, OVSK, Wireshark dissector
  git clone git://github.com/mininet/mininet
  sudo -iu vagrant mininet/util/install.sh -a
  #sudo -iu vagrant mininet/util/install.sh -tc

  ## Ryu
  pip install ipaddr networkx bitarray netaddr oslo.config routes webob \
    paramiko mock eventlet xml_compare pyflakes pylint pep8
  git clone https://github.com/osrg/ryu
  git clone https://bitbucket.org/sdnhub/ryu-starter-kit ryu/ryu/app/sdnhub_apps

  touch ~/runonce
fi
SCRIPT

Vagrant.configure("2") do |config|
  ## Box definition
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
  #    v.gui = true
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      v.customize ["modifyvm", :id, "--memory", "2048"]
  end

  ## Guest config
  config.vm.hostname = "sdnlab"
  config.vm.network :private_network, ip: "192.168.0.100"
  config.vm.network :forwarded_port, guest:6633, host:6634 # forwarding of port

  ## Provisioning
  config.vm.provision :shell, :inline => $init

  ## SSH config
  config.ssh.forward_x11 = true

end
