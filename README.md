Role Name
=========

Dummy role for testion role based scenarios in [ansible-core](https://github.com/tansudasli/ansible-core)

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Check (ansible-core)[https://github.com/tansudasli/ansible-core] repository for more details. Below is `run-role_dummy.yaml` content.

```
- name: Run the role
  hosts: localhost
  gather_facts: no
  
  vars:
    xyz: "zabidin"

    #  complex variable scenario
    # a)
    nodes:
      - name: master-node
        zone: europe-west4-a
        machine_type: f1-micro
        ips:
            - nic: 
                name: "ip-01"
      - name: worker-node
        zone: europe-west4-a
        machine_type: f1-micro
        ips:
          - nic: 
              name: "w-ip-01"

  tasks:
   
    - name: from playbook
      debug: 
        msg: "Message from playbook: {{xyz}}"

# in roles, all variables have to be passed from run playbook! So, you have to define the variables here!
# variables, in the role, under /vars or /default yaml files does not mean provides default values!
# variables, in the playbook, usage ways change the variable injection. So use a or b, not both
#   a) define a global variable on top of the playbook, and just define the role below
#   b) define variables under role below  
  roles: 
    # a)
    - {role: tansudasli.role_dummy}  #injects zabidin value from playbook' var section!
    # - {role: tansudasli.role_dummy, xyz: "abidin"} # injects from playbook but overrides zabidin as abidin!
    
    # b)
    # - role: tansudasli.role_dummy
    #   vars:
    #     nodes:
    #       - name: secondary-master-node
    #         zone: europe-west4-a
    #         machine_type: f1-micro
    #         ips:
    #           - nic: 
    #               name: "sm-ip-01"
    # - role: tansudasli.role_dummy
    #   vars:
    #   nodes:
    #     - name: worker-node-2
    #       zone: europe-west4-a
    #       machine_type: f1-micro
    #       ips:
    #         - nic: 
    #             name: "w2-ip-01"      

```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
