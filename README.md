
On Host
-------

```
sudo apt-get install qemu-system qemu-utils libvirt-bin libvirt-dev virt-manager virt-viewer python-spice-client-gtk libxslt-dev libxml2-dev libvirt-dev zlib1g-dev


vagrant plugin install vagrant-libvirt
vagrant plugin install hostmanager
```

```
export VAGRANT_DEFAULT_PROVIDER=libvirt
vagrant up
```

```
vagrant up --provider=libvirt
```
