#
# host resources
#
hosts = [
  {:name => "source", :cpus => 1, :mem => 1024, :promisc => [   ], :netmask => "255.255.255.0", :ip => ["192.168.33.10"]},
  {:name => "router", :cpus => 3, :mem => 1024, :promisc => [2,3], :netmask => "255.255.255.0", :ip => ["192.168.33.11","192.168.99.10"]},
  {:name => "sink",   :cpus => 1, :mem => 1024, :promisc => [2  ], :netmask => "255.255.255.0", :ip => ["192.168.99.11"]}
]

Vagrant.configure("2") do |config|

  # enable hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  # define each host
  hosts.each_with_index do |host, hostid|
    config.vm.define :node do |node|

      # host settings
      node.ssh.insert_key = "true"
      node.vm.hostname = host[:name]
      node.vm.box = "bento/centos-7.1"

      # define each network interface
      host[:ip].each do |ip|
        node.vm.network :private_network, ip: ip, netmask: host[:netmask]
      end

      # virtualbox settings - ignored if not applicable
      node.vm.provider "virtualbox" do |vb|
        vb.memory = host[:mem]
        vb.cpus = host[:cpus]

        # enable promisc mode on each specified nic
        host[:promisc].each do |nicid|
          vb.customize ["modifyvm", :id, "--nicpromisc#{nicid}", "allow-all"]
        end
      end

      # only execute the provisioner when all machines are up and running
      if hostid == hosts.length then
        config.vm.provision :ansible do |ansible|
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.extra_vars = {
            dpdk_device: ["enp0s8","enp0s9"],
            dpdk_target: "x86_64-native-linuxapp-gcc",
            num_huge_pages: 64
          }
        end
      end
    end
  end
end
