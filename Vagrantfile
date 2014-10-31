 -*- mode: ruby -*-
# vi: set ft=ruby :

$init = <<SCRIPT
if [ ! -f ~/runonce ]
then
  # Init
  git clone git://github.com/mininet/mininet
  sudo -iu vagrant mininet/util/install.sh -rtkr # Init
  sudo -iu vagrant mininet/util/install.sh -nb #f3 # Core
  sudo -iu vagrant mininet/util/install.sh -y # Ryu controller
  sudo -iu vagrant mininet/util/install.sh -mvV 2.3.0 # OVS(k)
  sudo -iu vagrant mininet/util/install.sh -cd # Cleanup
  # Make sure we don't run it again
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
