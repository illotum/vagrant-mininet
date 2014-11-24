# -*- mode: ruby -*-
# vi: set ft=ruby :

$init = <<SCRIPT
  aptitude update
  aptitude install -y build-essential fakeroot debhelper autoconf automake libssl-dev graphviz \
  python-all python-qt4 python-twisted-conch libtool git tmux vim python-pip python-paramiko \
  python-sphinx
  pip install alabaster
SCRIPT

$ovs = <<SCRIPT
  wget http://openvswitch.org/releases/openvswitch-2.3.0.tar.gz
  tar xf openvswitch-2.3.0.tar.gz
  pushd openvswitch-2.3.0
  DEB_BUILD_OPTIONS='parallel=8 nocheck' fakeroot debian/rules binary
  popd
  sudo dpkg -i openvswitch-common*.deb openvswitch-datapath-dkms*.deb python-openvswitch*.deb openvswitch-pki*.deb openvswitch-switch*.deb
  rm -rf *openvswitch*
SCRIPT

$mininet = <<SCRIPT
  git clone git://github.com/mininet/mininet
  pushd mininet
  git checkout -b 2.1.0p1 2.1.0p1
  patch -p0 < /vagrant/mn-options.patch
  ./util/install.sh -fn
  popd
SCRIPT

$ryu = <<SCRIPT
  aptitude install python-lxml python-pbr python-greenlet
  pip install six==1.7.0 networkx ryu
SCRIPT

$cleanup = <<SCRIPT
  aptitude clean
  rm -rf /tmp/*
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      v.customize ["modifyvm", :id, "--memory", "2048"]
  end

  ## Guest config
  config.vm.hostname = "sdnlab"
  config.vm.network :private_network, ip: "192.168.0.100"
  config.vm.network :forwarded_port, guest:6633, host:6634 # forwarding of port

  ## Provisioning
  config.vm.provision :shell, :inline => $init
  config.vm.provision :shell, privileged: false, :inline => $ovs
  config.vm.provision :shell, privileged: false, :inline => $mininet
  config.vm.provision :shell, :inline => $ryu
  config.vm.provision :shell, :inline => $cleanup

  ## SSH config
  config.ssh.forward_x11 = true

end
