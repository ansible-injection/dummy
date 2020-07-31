role-dummy
=========

Dummy role for testing role based scenarios in [ansible-core](https://github.com/tansudasli/ansible-core). This role also contains complex variable use-cases in Ansible roles.

Requirements
------------

- Install Ansible on your local or on a separate server. Run `brew install ansible`, then check `ansible --version`.

Role Variables
--------------

Only 2 type of variable have to be defined. 

- `xyz` is the simple var, 
- `nodes` is the complex var. This var is to simulate GCP infrastructure var definition. 

```
  vars:
    xyz: "zabidin"

    #  complex variable scenario
    # a)
    nodes:
      - name: standalone-node
        zone: europe-west4-a
        machine_type: f1-micro
        ips:
            - nic: 
                name: "sa-ip-01"

```

Dependencies
------------

n/a

Example Playbook
----------------

Below is `run-role_dummy.yaml` content in _ansible-core_ repository.

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

Apache-2.0 License

Author Information
------------------

[tansudasli](http://github.com/tansudasli)
