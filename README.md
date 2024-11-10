# Collecting AWS SSM Parameters by prefix as Ansible Facts

## Dependency:
- [amazon.aws](https://galaxy.ansible.com/ui/repo/published/amazon/aws/) `ansible-galaxy collection install amazon.aws`

## Install
```yaml
# requirements.yaml
collections:
  - name: amazon.aws
    version: 8.2.1

roles:
  - name: aws_ssm_export
    scm: git
    src: git@github.com:YouSysAdmin/ansible_aws_ssm_export.git
    version: v1.0.0
```

```shell
ansible-galaxy install -r requirements.yaml
```

## Usage
```yaml
# Export all params by prefix /ci/production

# AWS SSM params:
# /ci/production/instance_type
# /ci/production/instance_volume_size
# /ci/production/instance_user

# Result:
# ci_prod_aws_ssm:
#   instance_type: t3.medium
#   instance_volume_size: 50
#   instance_user: admin

# Usage vars
# vars:
#  instance_type: "{{ ci_aws_ssm.instance_type }}"

# Exporting values from the AWS SSM as global Ansible facts
- hosts: all
  gather_facts: False
  run_once: true
  vars:
    ssm_prefixes:
    - name: ci_prod
      prefix: '/ci/production'
  roles:
    - aws_ssm_export
  tags: [ "always" ]

- name: other tasks
  ...
```


