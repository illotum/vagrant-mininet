# -*- mode: ruby -*-
# vi: set ft=ruby :

$init = <<SCRIPT
if [ ! -f ~/runonce ]
then
  ## Init
  locale-gen en_CA.UTF-8 # Feel free to change it to your locale
  sed -i -e 's/\/archive.ubuntu.com/\/mirrors.kernel.org/' /etc/apt/sources.list
  aptitude update
  aptitude install -yq git-core libpcap-dev libsqlite3-dev gcc make

  ## Mininet, POX, OVSK, Wireshark dissector
  git clone git://github.com/mininet/mininet
  sudo -iu vagrant mininet/util/install.sh
  sudo -iu vagrant mininet/util/install.sh -tc

  ## Trema
  aptitude install -yq gcc make ruby rubygems ruby-dev libpcap-dev libsqlite3-dev \
     libglib2.0-dev flex bison libgtkmm-2.4-dev
  ## Option: gem
  gem install trema --no-ri --no-rdoc
  ## Option: fom source
  # gem install bundler --no-ri --no-rdoc
  # git clone https://github.com/trema/trema
  # wget http://wiresharkdownloads.riverbed.com/wireshark/src/wireshark-1.8.10.tar.bz2
  # tar xf wireshark-1.8.10.tar.bz2 && rm wireshark-1.8.10.tar.bz2
  # cd trema
  # git checkout master
  # bundle install
  # ./build.rb
  # cd src/tremashark/plugin/
  # ln -s ~/wireshark-1.8.10 wireshark
  # cd wireshark && ./configure
  # cd ../packet-trema && make
  # sudo -iu vagrant mkdir -p ~/.wireshark/plugins
  # sudo -iu vagrant cp packet-trema.so ~/.wireshark/plugins
  # sudo -iu vagrant cp ../user_dlts ~/.wireshark/


  ## FlowVisor
  wget http://updates.onlab.us/GPG-KEY-ONLAB
  apt-key add GPG-KEY-ONLAB
  echo "deb http://updates.onlab.us/debian stable/" >> /etc/apt/sources.list
  aptitude update
  aptitude install -yq flowvisor
  #sudo -u flowvisor fvconfig generate /dev/null

  ## Clojure
  #aptitude install -yq libg2-xpm openjdk-7-jdk
  #wget -O /usr/local/bin/lein https://raw.github.com/technomancy/leiningen/stable/bin/lein
  #chmod 755 /usr/local/bin/lein

  touch ~/runonce
fi
SCRIPT

Vagrant.configure("2") do |config|
  ## Box definition
  config.vm.box = "raring64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/raring/current/raring-server-cloudimg-vagrant-amd64-disk1.box"
  #config.vm.box = "raring32"
  #config.vm.box_url = "http://cloud-images.ubuntu.com/raring/current/raring-server-cloudimg-vagrant-i386-disk1.box"

  ## VM config (optional)
  #config.vm.provider "virtualbox" do |v|
  #    v.gui = true
  #    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
  #    v.customize ["modifyvm", :id, "--memory", "2048"]
  #end

  ## Guest config
  config.vm.hostname = "mininet"
  config.vm.network :private_network, ip: "192.168.33.253"
  config.vm.network :forwarded_port, guest:6633, host:6634 # forwarding controller port

  ## Provisioning
  config.vm.provision :shell, :inline => $init

  ## SSH config
  config.ssh.forward_x11 = true

end
