# virt manager

# network groups

```
sudo virsh net-list
```

## starting one

```
sudo virsh net-start <name>
```

# see the details of a network group

```
sudo virsh net-dhcp-leases default
```

# see VMs list

```
sudo virsh list --all
```

# see the mac address

```
sudo virsh domiflist <vm_name>
```
