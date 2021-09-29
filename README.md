Manage AWS EC2 Snapshots
=========

This role creates snapshots for AWS EC2 instances with EBS disks.


Role Variables
--------------

```yaml

# not required. instance that has the required volume to snapshot mounted. One of either ec2_snapshot_volume_id or  ec2_snapshot_instance_id should be used. If ec2_snapshot_instance_id is used then either device_name or ec2_snapshot_all_volumes: true must be used.
ec2_snapshot_instance_id: 
# not required. description to be applied to the snapshot
ec2_snapshot_description: Managed by Ansible Automation Platform
# not required. volume from which to take the snapshot. One of either volume_id or device_name must be provided.
ec2_snapshot_volume_id:
# Required. device name of a mounted volume to be snapshotted. One of either volume_id or device_name must be provided.
ec2_snapshot_device_name:
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
# # not required. AWS access key. If not set then the value of the AWS_ACCESS_KEY_ID, AWS_ACCESS_KEY or EC2_ACCESS_KEY environment variable is used.
ec2_snapshot_secret_key: 

# If set to true, all EBS volumes attached to an EC2 instance will have a snapshot created. If false then the ec2_snapshot_device_name variable must be provided with the ec2_snapshot_instance_id variable 
# OR alternatively provide the ec2_snapshot_volume_id variable (without the ec2_snapshot_instance_id variable and ec2_snapshot_device_name variable) of the volume which is to have a snapshot created for.
ec2_snapshot_all_volumes: true

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
