# The Fedora Atomic Seed Images

Bootable container images that represent a stream of software updates
that are tested and can be used as inputs for layered container builds
to easily create many different Operating system variants.

## Building

To build locally simply run:

```
podman build --security-opt=label=disable --cap-add=all --device /dev/fuse -t localhost/fedora-atomic-seed:latest .
```

To build a disk image using `image-builder` CLI:

```
sudo image-builder build                            \
    --output-dir ./output                           \
    --bootc-ref localhost/fedora-atomic-seed:latest \
    --bootc-default-fs xfs qcow2
```

To launch with libvirt:

```
virt-install --import --name seed --ram=1024 \
    --osinfo detect=on,require=off --autoconsole=text --disk=output/disk.qcow2
```

