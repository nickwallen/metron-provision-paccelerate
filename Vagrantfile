

Vagrant.configure("2") do |config|

  # enable hostmanager
  # config.hostmanager.enabled = true
  # config.hostmanager.manage_host = true

  config.vm.define :node do |node|

    # host settings
    node.vm.hostname = "dpdk1"
    node.vm.box = "centos/7"
    node.vm.network :private_network, ip: "192.168.3.3"
    node.ssh.insert_key = "true"

    # provider settings
    node.vm.provider :libvirt do |libvirt|
        libvirt.memory = 2048
        libvirt.cpus = 2
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
