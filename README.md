# The Fedora Atomic Seed Images

Bootable container images that represent a stream of software updates
that are tested and can be used as inputs for layered container builds
to easily create many different Operating system variants.

## Building

To build locally simply run:

```
sudo podman build --security-opt=label=disable --cap-add=all --device /dev/fuse -t localhost/fedora-atomic-seed:latest .
```

To build a disk image using `image-builder` CLI:

```
sudo dnf install -y image-builder
sudo image-builder build                            \
    --output-dir ./output --output-name disk        \
    --bootc-ref localhost/fedora-atomic-seed:latest \
    --bootc-default-fs xfs qcow2
```

This will output something like:

```
Image build successful: output/disk.qcow2
```

## Testing

To launch with libvirt and embed an SSH key for `root` user:

```
KEY_B64=$(base64 -w0 < /path/to/sshkey.pub)
virt-install --import --name seed --ram=1024                \
    --osinfo detect=on,require=off --autoconsole=text       \
    --boot=uefi,bootmenu.enable=on,bios.useserial=on        \
    --network bridge=virbr0,model=virtio                    \
    --disk=size=20,backing_store="${PWD}/output/disk.qcow2" \
    --qemu-commandline="-smbios type=11,value=io.systemd.credential.binary:ssh.authorized_keys.root=${KEY_B64}"
```

To detect the IP address assigned to the instance:

```
virsh domifaddr seed  --full --source=arp
```

Then connect via SSH:

```
ssh root@192.168.122.132
```
