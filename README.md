# ICSE Virtual Server Module

This module creates a virtual server on a single subnet within a VPC. Users can optionally create block storage volumes and attach floating IPs

## Module Variables

Name                             | Type                                                                                                                                                                           | Description                                                                                                                                                                             | Sensitive | Default
-------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | -------
prefix                           | string                                                                                                                                                                         | The prefix that you would like to prepend to your resources                                                                                                                             |           | 
tags                             | list(string)                                                                                                                                                                   | List of Tags for the resource created                                                                                                                                                   |           | null
resource_group_id                | string                                                                                                                                                                         | Resource group ID for the VSI                                                                                                                                                           |           | null
zone                             | string                                                                                                                                                                         | Zone where the VSI and Block Storage will be provisioned                                                                                                                                |           | 
vpc_id                           | string                                                                                                                                                                         | ID of the VPC where VSI will be provisioned                                                                                                                                             |           | 
primary_subnet_id                | string                                                                                                                                                                         | ID of the subnet where the VSI will be provisioned.                                                                                                                                     |           | 
primary_security_group_ids       | list(string)                                                                                                                                                                   | (Optional) List of security group ids to add to the primary network interface. Using an empty list will assign the default VPC security group.                                          |           | []
secondary_subnets                | list( object({ name = string id = string security_groups = optional(list(string)) allow_ip_spoofing = optional(bool) }) )                                                      | (Optional) List of secondary subnets to connect to the instance                                                                                                                         |           | []
name                             | string                                                                                                                                                                         | Name of the server instance                                                                                                                                                             |           | 
image_id                         | string                                                                                                                                                                         | ID of the server image to use for VSI creation                                                                                                                                          |           | 
profile                          | string                                                                                                                                                                         | Type of machine profile for VSI. Use the command `ibm is instance-profiles` to find available profiles in your region                                                                   |           | bx2-2x8
ssh_key_ids                      | list(string)                                                                                                                                                                   | List of SSH Key Ids. At least one SSH key must be provided                                                                                                                              |           | 
boot_volume_encryption_key       | string                                                                                                                                                                         | (Optional) Boot volume encryption key                                                                                                                                                   |           | null
user_data                        | string                                                                                                                                                                         | (Optional) Data to transfer to instance                                                                                                                                                 |           | null
allow_ip_spoofing                | bool                                                                                                                                                                           | Allow IP spoofing on primary network interface                                                                                                                                          |           | false
add_floating_ip                  | bool                                                                                                                                                                           | Add a floating IP to the primary network interface.                                                                                                                                     |           | false
block_storage_volumes            | list( object({ name = string profile = string capacity = optional(number) iops = optional(number) encryption_key = optional(string) delete_all_snapshots = optional(bool) }) ) | List describing the block storage volumes that will be attached to the VSI. Storage volumes will be provisoned in the same resource group as the server instance.                       |           | []
secondary_floating_ips           | list(string)                                                                                                                                                                   | List of secondary interfaces to add floating ips                                                                                                                                        |           | []
availability_policy_host_failure | string                                                                                                                                                                         | (Optional) The availability policy to use for this virtual server instance. The action to perform if the compute host experiences a failure. Supported values are `restart` and `stop`. |           | null
boot_volume_name                 | string                                                                                                                                                                         | (Optional) Name of the boot volume                                                                                                                                                      |           | null
boot_volume_size                 | number                                                                                                                                                                         | (Optional) The size of the boot volume.(The capacity of the volume in gigabytes. This defaults to minimum capacity of the image and maximum to 250                                      |           | null
dedicated_host                   | string                                                                                                                                                                         | (Optional) The placement restrictions to use the virtual server instance. Unique ID of the dedicated host where the instance id placed.                                                 |           | null
dedicated_host_group             | string                                                                                                                                                                         | (Optional) The placement restrictions to use for the virtual server instance. Unique ID of the dedicated host group where the instance is placed.                                       |           | null
default_trusted_profile_target   | string                                                                                                                                                                         | (Optional) The unique identifier or CRN of the default IAM trusted profile to use for this virtual server instance.                                                                     |           | null
metadata_service_enabled         | bool                                                                                                                                                                           | (Optional) Indicates whether the metadata service endpoint is available to the virtual server instance. Default value : false                                                           |           | null
placement_group                  | string                                                                                                                                                                         | (Optional) Unique Identifier of the Placement Group for restricting the placement of the instance                                                                                       |           | null

## Module Outputs

Name                   | Description
---------------------- | ---------------------------------------
id                     | Virtual Server ID
name                   | Virtual Server name
primary_ipv4_address   | Primary ipv4 address for virtual server
floating_ip            | Floating IP if created
secondary_floating_ips | List of secondary floating IPs


## Example Usage

```terraform
module vsi_module {
  source                           = "github.com/Cloud-Schematics/icse-vsi-module"
  prefix                           = var.prefix
  tags                             = var.tags
  resource_group_id                = var.resource_group_id
  zone                             = var.zone
  vpc_id                           = var.vpc_id
  primary_subnet_id                = var.primary_subnet_id
  primary_security_group_ids       = var.primary_security_group_ids
  secondary_subnets                = var.secondary_subnets
  name                             = var.name
  image_id                         = var.image_id
  profile                          = var.profile
  ssh_key_ids                      = var.ssh_key_ids
  boot_volume_encryption_key       = var.boot_volume_encryption_key
  user_data                        = var.user_data
  allow_ip_spoofing                = var.allow_ip_spoofing
  add_floating_ip                  = var.add_floating_ip
  block_storage_volumes            = var.block_storage_volumes
  secondary_floating_ips           = var.secondary_floating_ips
  availability_policy_host_failure = var.availability_policy_host_failure
  boot_volume_name                 = var.boot_volume_name
  boot_volume_size                 = var.boot_volume_size
  dedicated_host                   = var.dedicated_host
  dedicated_host_group             = var.dedicated_host_group
  default_trusted_profile_target   = var.default_trusted_profile_target
  metadata_service_enabled         = var.metadata_service_enabled
  placement_group                  = var.placement_group
}
```