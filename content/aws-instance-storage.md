!!! note
    This feature is available with bosh-aws-cpi v32+ and only for releases deployed with ? stemcells.

Certain [instance types](https://aws.amazon.com/ec2/instance-types/) have access to instance storage. All BOSH managed VMs have to store some ephemeral data such as release jobs, packages, logs and other scratch data. First instance storage disk is used if possible; otherwise, a separate EBS volume is created as an ephemeral disk.

Applications may need access to all instance storage disks. In that case separate EBS volume will always be created to store ephemeral data. To enable access to all instance storage disks add `raw_instance_storage: true`:

```yaml
resource_pools:
- name: default
  network: default
  stemcell:
    name: bosh-aws-xen-hvm-ubuntu-xenial-go_agent
    version: latest
  cloud_properties:
    instance_type: d2.2xlarge
    raw_instance_storage: true
```

With multiple disks attached, the Agent partitions and labels instance storage disks with label `raw-ephemeral-*` so that release jobs can easily find and use them:

```shell
ls -la /dev/disk/by-partlabel/raw-ephemeral-*
```

```text
lrwxrwxrwx 1 root root 12 Oct  5 03:09 /dev/disk/by-partlabel/raw-ephemeral-0 -> ../../xvdba1
lrwxrwxrwx 1 root root 12 Oct  5 03:09 /dev/disk/by-partlabel/raw-ephemeral-1 -> ../../xvdbb1
```
