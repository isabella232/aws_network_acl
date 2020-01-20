# aws_network_acl

Master: [![Build Status](https://travis-ci.org/sansible/aws_network_acl.svg?branch=master)](https://travis-ci.org/sansible/aws_network_acl)
Develop: [![Build Status](https://travis-ci.org/sansible/aws_network_acl.svg?branch=develop)](https://travis-ci.org/sansible/aws_network_acl)

* [Installation and Dependencies](#installation-and-dependencies)
* [Role Variables](#role-variables)
* [Examples](#examples)

This role will create an AWS Network ACL using Cloudformation.

## Installation and Dependencies

To install run `ansible-galaxy install sansible.aws_network_acl` or add this to your
`roles.yml`.

```YAML
- name: sansible.aws_network_acl
  version: v1.1
```

and run `ansible-galaxy install -p ./roles -r roles.yml`

## Role Variables

|Variable|Default|Description|
|---|---|---|
|sansible_aws_network_acl_rules|[]|List of hashes for ACL rules|
|sansible_aws_network_acl_rules.action|allow|Action to take based on rule|
|sansible_aws_network_acl_rules.cidr|~|CIDR for ACL rule|
|sansible_aws_network_acl_rules.direction|~|Direction for, can be egress, ingress or both|
|sansible_aws_network_acl_rules.port|~|Port for ACL rule|
|sansible_aws_network_acl_rules.port_to|port|Port to for the ACL, if left blank port is used|
|sansible_aws_network_acl_rules.protocol|~|Protocol for ACL rule, can be tcp or udp|
|sansible_aws_network_acl_rules.rule_number|~|Rule number for ACL rule, for rules that have a direction of both leave a gap between rules so allow for an egress and ingress rule|
|sansible_aws_network_acl_region|~|AWS region for the NACL|
|sansible_aws_network_acl_stack_name|~|Name for NACL CF stack|
|sansible_aws_network_acl_subnet_ids|~|Subnets to attach NACL to|
|sansible_aws_network_acl_tags|~|Tags for the NACL, you must specify a Name tag|
|sansible_aws_network_acl_vpc_id|~|VPC to attach NACL to|

## Examples

Simply include role in your playbook:

```YAML
- name: Install aws_network_acl
  hosts: somehost

  roles:
    - role: aws_network_acl
      sansible_aws_network_acl_rules:
        - port: 22
          rule_number: 100
          protocol: 'tcp'
          cidr: 0.0.0.0/0
          direction: both
        - port: 80
          rule_number: 105
          protocol: 'tcp'
          cidr: 0.0.0.0/0
          direction: both
        - port: 123
          rule_number: 110
          protocol: 'udp'
          cidr: 0.0.0.0/0
          direction: both
        - port: 443
          rule_number: 115
          protocol: 'tcp'
          cidr: 0.0.0.0/0
          direction: both
        - port: 1024
          port_to: 65535
          rule_number: 120
          protocol: 'tcp'
          cidr: 0.0.0.0/0
          direction: both
        - port: 1024
          port_to: 65535
          rule_number: 125
          protocol: 'udp'
          cidr: 0.0.0.0/0
          direction: both
      sansible_aws_network_acl_region: eu-west-1
      sansible_aws_network_acl_stack_name: dev-network-acl
      sansible_aws_network_acl_subnet_ids:
        - some-subnet-id-1
        - some-subnet-id-2
      sansible_aws_network_acl_tags:
        Name: dev-network-acl
      sansible_aws_network_acl_vpc_id: some-vpc-id
```

