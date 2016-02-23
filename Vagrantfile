#
# Hosts:
# => source: spews out previously captured network traffic onto 192.168.33.x
# => router: captures traffic from 192.168.33.x and forwards to 192.168.99.x
# => sink: captures traffic on 192.168.99.x
#
# NOTE: May need to increase number of huge pages to 512. The 'testpmd' application
# will fail to start if there are not enough huge pages.
#
# On 'sink':
#     $ tcpdump -i enp0s8
#
# On 'router'
#     $ printf "27 \n" | $RTE_SDK/tools/setup.sh
#     testpmd> show port info all
#     testpmd> start
#     testpmd> show port stats all
#     testpmd> stop
#
Vagrant.configure("2") do |config|

  # enable hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  # source
  config.vm.define "source" do |node|
    node.vm.hostname = "source"
    node.vm.box = "bento/centos-7.1"
    node.vm.network :private_network, ip: "192.168.33.10", netmask: "255.255.255.0"
    node.ssh.insert_key = "true"
    node.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    node.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

  # router
  config.vm.define "router" do |node|
    node.vm.hostname = "router"
    node.vm.box = "bento/centos-7.1"
    node.vm.network :private_network, ip: "192.168.33.11", netmask: "255.255.255.0"
    node.vm.network :private_network, ip: "192.168.99.10", netmask: "255.255.255.0"
    node.ssh.insert_key = "true"
    node.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 3
      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
    end
    node.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

  # sink
  config.vm.define "sink" do |node|
    node.vm.hostname = "sink"
    node.vm.box = "bento/centos-7.1"
    node.vm.network :private_network, ip: "192.168.99.11", netmask: "255.255.255.0"
    node.ssh.insert_key = "true"
    node.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    end
    node.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end
end
