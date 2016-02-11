#
# works on either virtualbox or libvirt
#

# host resources
host = {:hostname => "dpdk1", :cpus => 2, :memory => 2048}


Vagrant.configure("2") do |config|

  # enable hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.define :node do |node|

    # host settings
    node.vm.hostname = host[:hostname]
    node.vm.box = "centos/7"
    node.vm.network :private_network, ip: "192.168.3.3"
    node.ssh.insert_key = "true"

    # libvirt settings - ignored if not applicable
    node.vm.provider "libvirt" do |vt|
        vt.memory = host[:memory]
        vt.cpus = host[:cpus]
    end

    # virtualbox settings - ignored if not applicable
    node.vm.provider "virtualbox" do |vb|
      vb.memory = host[:memory]
      vb.cpus = host[:cpus]
    end

    # provisioning with ansible
    node.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.extra_vars = {
        dpdk_device: "eth1",
        dpdk_version: "2.2.0",
        dpdk_target: "x86_64-ivshmem-linuxapp-gcc",
        num_huge_pages: 512
      }
    end
  end
end
