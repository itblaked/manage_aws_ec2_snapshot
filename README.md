Manage AWS EC2 Snapshots
=========

This role creates snapshots for AWS EC2 instances with EBS disks.

Snapshots are performed on a per volume basis.

To perform a snapshot, the role needs to know either the unique `volume_id` or the `instance_id` and the `device_name`. These can be provided using the variables `ec2_snapshot_volume_id`, `ec2_snapshot_instance_id` and `ec2_snapshot_device_name`.

Typically EC2 instances have only one volume with the device name `/dev/sda1`, so this is set as the default value for the device name.
If the `instance_id` or `volume_id` are provided, then the role will create a snapshot on the specified volume or device for the specified instance.

To snapshot all volumes on an instance, use the `ec2_snapshot_all_volumes` variable. The role will discover the device_name of each of the volumes of an instance and create a snapshot of them.

Enterprise EC2 instances often have the system hostname or fqdn set as the value of the `Name` tag and Ansible inventories often have systems listed by the hostname or fqdn. This role is able to determine the ec2_instance_id of a managed system from the inventory by searching for the instance where the tag `Name` has the same value as the system in the Ansible inventory. 

**Example**:

EC2 Instance ID `i-0a1234bcde567fgh8` has the tag `Name: server1.example.com` and the server is in the Ansible inventory as `server1.example.com`. 

Two variables control this feature: 

The variable `ec2_discover_instance_id_with_filter` determines whether to attempt to search for the instance and the `ec2_snapshot_instance_filter` variable determines which filter to use to search for the instance.

The default values for these two variables is:
```yaml
ec2_discover_instance_id_with_filter: true

ec2_snapshot_instance_filter:
  "tag:Name": "{{ inventory_hostname }}"

```

The result of these defaults is that the Ansible Inventory hostname value will be used to search for the instance against the tag `Name`.

**Note**: Dynamic discovery of the instance_id will not be used if  `ec2_snapshot_instance_id` or `ec2_snapshot_volume_id` is provided, they will be used instead to snapshot a single device.

Role Variables
--------------


```yaml

# not required. instance that has the required volume to snapshot mounted. 
# NOTE: One of either ec2_snapshot_volume_id or ec2_snapshot_instance_id can be used. 
# If ec2_snapshot_instance_id is used then either device_name or ec2_snapshot_all_volumes: true must also be used.
ec2_snapshot_instance_id: 

# not required. description to be applied to the snapshot
ec2_snapshot_description: Managed by Ansible Automation Platform

# not required. A dict of filters to apply. Each dict item consists of a filter key and a filter value. See U(https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html) for possible filters. Filter names and values are case sensitive.
# Default is to search for the inventory_hostname of the target host in the 'Name' tag of EC2.
ec2_snapshot_instance_filter:
  "tag:Name": "{{ inventory_hostname }}"

# not required. volume from which to take the snapshot. One of either volume_id or device_name must be provided.
ec2_snapshot_volume_id:

# Required. device name of a mounted volume to be snapshotted. One of either volume_id or device_name must be provided.
ec2_snapshot_device_name: /dev/sda1

# Required. choices: absent;present. whether to add or create a snapshot. Default is absent
ec2_snapshot_state: present

# not required. a hash/dictionary of tags to add to the snapshot. Defaults to 'managed_by_aap: true' where aap is Ansible Automation Platform
ec2_snapshot_tags:
  managed_by_aap: true

# not required. The AWS region to use. If not specified then the value of the AWS_REGION or EC2_REGION environment variable, if any, is used. See U(http://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region)
ec2_snapshot_aws_region:

# not required. When set to "no", SSL certificates will not be validated for boto versions >= 2.6.0. Default is True
ec2_snapshot_validate_certs: True

# not required. AWS STS security token. If not set then the value of the AWS_SECURITY_TOKEN or EC2_SECURITY_TOKEN environment variable is used.
ec2_snapshot_security_token:

# not required. AWS secret key. If not set then the value of the AWS_SECRET_ACCESS_KEY, AWS_SECRET_KEY, or EC2_SECRET_KEY environment variable is used.
ec2_snapshot_access_key: 

# not required. AWS access key. If not set then the value of the AWS_ACCESS_KEY_ID, AWS_ACCESS_KEY or EC2_ACCESS_KEY environment variable is used.
ec2_snapshot_secret_key: 

# If set to true, all EBS volumes attached to an EC2 instance will have a snapshot created. If false then the ec2_snapshot_device_name variable must be provided with the ec2_snapshot_instance_id variable 
# OR alternatively provide the ec2_snapshot_volume_id variable (without the ec2_snapshot_instance_id variable and ec2_snapshot_device_name variable) of the volume which is to have a snapshot created for.
ec2_snapshot_all_volumes: true

# If set to true, then the instance_id will be discovered using the ec2_snapshot_instance_filter variable.
ec2_discover_instance_id_with_filter: true

```
Example Playbook
----------------


```yaml
- name: Manage AWS EC2 Snapshots
  hosts: all
  gather_facts: no
  tasks:
    - name: Apply Manage AWS EC2 Snapshots role
      include_role:
        name: manage_aws_ec2_snapshot
```


License
-------

BSD

Author Information
------------------

Blake Douglas, blake@redhat.com
