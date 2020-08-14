# README aws_cloudformation_stack_set* Ansible modules

These modules are meant to go to Ansible community repo, when I have the time.

Here is some example how to use them.

## aws_cloudformation_stack_set

To create a stackset with template provided in a file called `mycfn.yml`:

```
- name: Create stackset
  aws_cloudformation_stack_set:
    stack_set_name: mycfn
    template: mycfn.yml
    state: present
```

To delete the same stackset:

```
- name: Delete stackset
  aws_cloudformation_stack_set:
    stack_set_name: mycfn
    state: absent
```

To create a stackset that can be deployed against organizational unit(s) with automatic deployment to accounts under OU(s):

```
- name: Create stackset
  aws_cloudformation_stack_set:
    stack_set_name: mycfn
    template: mycfn.yml
    state: present
    permission_model: SERVICE_MANAGED
    capabilities:
      - CAPABILITY_NAMED_IAM
    auto_deployment:
      enabled: True
      retain_stacks_on_account_removal: False
```

## aws_cloudformation_stack_set_info

To get basic information about existing (active) stackset:

```
- name: Get all stackset info
  aws_cloudformation_stack_set_info:
  register: output
- name: Print output
  debug:
    msg: "{{ output }}"
```

To get information about a specific stackset and all the instances:

```
- name: Get all stackset info
  aws_cloudformation_stack_set_info:
    stack_set_name: mycfn
    stack_instances: Yes
  register: output
- name: Print output
  debug:
    msg: "{{ output }}"
```

## aws_cloudformation_stack_set_instance

To deploy a stackset to a list of accounts:

```
- name: Deploy stack instances
  aws_cloudformation_stack_set_instance:
    stack_set_name: mycfn
    accounts:
      - '123456789012'
      - '345678901234'
    regions:
      - us-west-1
    state: present
```

Or using deployment targets:

```
- name: Deploy stack instances
  aws_cloudformation_stack_set_instance:
    stack_set_name: mycfn
    deployment_targets:
      accounts:
        - '123456789012'
        - '345678901234'
    regions:
      - us-west-1
    state: present
```

To create stack instances against Organizational Unit(s):

```
- name: Deploy stack instances
  aws_cloudformation_stack_set_instance:
    stack_set_name: mycfn
    deployment_targets:
      organizational_unit_ids:
        - ou-abcd-ef12gh34ij56
        - ou-zyxw-ef12gh34ij56
    regions:
      - us-west-1
    state: present
```

To delete the stack instances, just change state to absent:

```
- name: Remove stack instances
  aws_cloudformation_stack_set_instance:
    stack_set_name: mycfn
    deployment_targets:
      organizational_unit_ids:
        - ou-abcd-ef12gh34ij56
        - ou-zyxw-ef12gh34ij56
    regions:
      - us-west-1
    state: absent
```

