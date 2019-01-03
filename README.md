Role Name
=========

This role creates Certificate Authority certificate and key on 'localhost' and signs per host specific certificate.
These signed certificates and private keys (along with Certificate Sign Request) are copied into given node.

See default variables for details like used password, locations, subject/issuer information etc.

Requirements
------------

Ansible 2.7+

Usage
------

Use this role with a playbook defined like this:

```yaml
- name: Create CA certificates
  hosts: localhost
  tasks:
    - name: Prepare node dependencies
      when: amq_ssl_certs_self_signed is defined and amq_ssl_certs_self_signed == False
      include_tasks: roles/ansible-ssl-generation/tasks/install_dependencies.yml

    - name: Generate local CA
      when: amq_ssl_certs_self_signed is defined and amq_ssl_certs_self_signed == False
      include_tasks: roles/ansible-ssl-generation/tasks/generate_root_ca.yml

- name: Generate self-signed SSL keys
  hosts: all
  gather_facts: True
  roles:
    - ansible-ssl-generation
```

This role is to be used with [ansible-broker-clusters/playbooks/gen-ssl](https://github.com/msgqe/ansible-broker-clusters/tree/master/playbooks/gen-ssl) for start.
