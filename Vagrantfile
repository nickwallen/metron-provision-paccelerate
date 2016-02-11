#
# works on either virtualbox or libvirt
#

# host resources
cpus = 2
mem = 2048

Vagrant.configure("2") do |config|

  # enable hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.define :node do |node|

    # host settings
    node.vm.hostname = "dpdk1"
    node.vm.box = "centos/7"
    node.vm.network :private_network, ip: "192.168.3.3"
    node.ssh.insert_key = "true"

    # libvirt provider settings - ignored if not applicable
    node.vm.provider :libvirt do |vt|
        vt.memory = mem
        vt.cpus = cpus
    end

    # virtualbox provider settings - ignored if not applicable
    node.vm.provider "virtualbox" do |vb|
      vb.memory = mem
      vb.cpus = cpus
    end

    # provisioning with ansible
    node.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.extra_vars = {
        dpdk_version: "2.2.0",
        dpdk_target: "x86_64-ivshmem-linuxapp-gcc"
      }
    end
  end
end
