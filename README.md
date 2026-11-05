# The Fedora Atomic Seed Images

Bootable container images that represent a stream of software updates
that are tested and can be used as inputs for layered container builds
to easily create many different Operating system variants.

## Building

To build locally simply run:

```
podman build --security-opt=label=disable --cap-add=all --device /dev/fuse -t localhost/fedora-atomic-seed:latest .
```

To run in a test VM use `bcvk`:

```
bcvk libvirt run --name fedora-atomic-seed-vm localhost/fedora-atomic-seed:latest
```
