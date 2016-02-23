Virtualized Fast Packet Processing
==================================

Provisions a host using Vagrant and Ansible with the Data Plane Development Kit ([DPDK](http://dpdk.org/)).  This host can then be used to launch fast packet processing applications that leverage DPDK in a virtualized environment.

Getting Started
---------------

### Virtualbox

```
ansible-galaxy install -r requirements.yml
vagrant up 
```

### Validation

Once the host has been provisioned, the following commands can be used to validate that the network device has been correctly bound to DPDK.

```
vagrant ssh
export RTE_SDK=/opt/dpdk-2.2.0
export RTE_TARGET=x86_64-ivshmem-linuxapp-gcc
sudo ${RTE_SDK}/${RTE_TARGET}/app/testpmd -c 0x03 -n 4 -- -i
```

```
# sudo ${RTE_SDK}/${RTE_TARGET}/app/testpmd -c 0x03 -n 4 -- -i
  EAL: Detected lcore 0 as core 0 on socket 0
  EAL: Detected lcore 1 as core 1 on socket 0
  EAL: Support maximum 128 logical core(s) by configuration.
  EAL: Detected 2 lcore(s)
  EAL: VFIO modules not all loaded, skip VFIO support...
  EAL: Searching for IVSHMEM devices...
  EAL: No IVSHMEM configuration found!
  EAL: Setting up physically contiguous memory...
  EAL: Ask a virtual area of 0x800000 bytes
  EAL: Virtual area found at 0x7f1ac6400000 (size = 0x800000)
  EAL: Ask a virtual area of 0x200000 bytes
  EAL: Virtual area found at 0x7f1ac6000000 (size = 0x200000)
  EAL: Ask a virtual area of 0xf000000 bytes
  EAL: Virtual area found at 0x7f1ab6e00000 (size = 0xf000000)
  EAL: Ask a virtual area of 0x19200000 bytes
  EAL: Virtual area found at 0x7f1a9da00000 (size = 0x19200000)
  EAL: Ask a virtual area of 0x400000 bytes
  EAL: Virtual area found at 0x7f1a9d400000 (size = 0x400000)
  EAL: Ask a virtual area of 0x1a00000 bytes
  EAL: Virtual area found at 0x7f1a9b800000 (size = 0x1a00000)
  EAL: Ask a virtual area of 0x1800000 bytes
  EAL: Virtual area found at 0x7f1a99e00000 (size = 0x1800000)
  EAL: Ask a virtual area of 0x7800000 bytes
  EAL: Virtual area found at 0x7f1a92400000 (size = 0x7800000)
  EAL: Ask a virtual area of 0x5200000 bytes
  EAL: Virtual area found at 0x7f1a8d000000 (size = 0x5200000)
  EAL: Ask a virtual area of 0x400000 bytes
  EAL: Virtual area found at 0x7f1a8ca00000 (size = 0x400000)
  EAL: Ask a virtual area of 0x200000 bytes
  EAL: Virtual area found at 0x7f1a8c600000 (size = 0x200000)
  EAL: Ask a virtual area of 0x800000 bytes
  EAL: Virtual area found at 0x7f1a8bc00000 (size = 0x800000)
  EAL: Requesting 461 pages of size 2MB from socket 0
  EAL: TSC frequency is ~2494226 KHz
  EAL: Master lcore 0 is ready (tid=c7ea8900;cpuset=[0])
  EAL: lcore 1 is ready (tid=8b3fe700;cpuset=[1])
  EAL: PCI device 0000:00:03.0 on NUMA socket -1
  EAL:   probe driver: 8086:100e rte_em_pmd
  EAL:   Not managed by a supported kernel driver, skipped
  EAL: PCI device 0000:00:08.0 on NUMA socket -1
  EAL:   probe driver: 8086:100e rte_em_pmd
  EAL:   PCI memory mapped at 0x7f1ac6c00000
  PMD: eth_em_dev_init(): port_id 0 vendorID=0x8086 deviceID=0x100e
  Interactive-mode selected
  Configuring Port 0 (socket 0)
  PMD: eth_em_tx_queue_setup(): sw_ring=0x7f1a9d5edb80 hw_ring=0x7f1a9d5efc80 dma_addr=0x2adefc80
  PMD: eth_em_rx_queue_setup(): sw_ring=0x7f1a9d5dd640 hw_ring=0x7f1a9d5ddb40 dma_addr=0x2adddb40
  PMD: eth_em_start(): <<
  Port 0: 08:00:27:DF:C9:8E
  Checking link statuses...
  Port 0 Link Up - speed 1000 Mbps - full-duplex
  Done
testpmd> show port stats all

  ######################## NIC statistics for port 0  ########################
  RX-packets: 0          RX-missed: 0          RX-bytes:  0
  RX-errors: 0
  RX-nombuf:  0         
  TX-packets: 0          TX-errors: 0          TX-bytes:  0
  ############################################################################
```
