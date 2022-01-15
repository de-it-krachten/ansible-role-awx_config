[![CI](https://github.com/de-it-krachten/ansible-role-awx_config/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-awx_config/actions?query=workflow%3ACI)


# ansible-role-awx_config

AWX configuration (configuration-as-code)


Platforms
--------------

Supported platforms

- CentOS 7
- CentOS 8
- RockyLinux 8
- AlmaLinux 8
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS



Role Variables
--------------
<pre><code>
# List of organization
awx_organizations: []

# path which organizational configs
awx_organization_path: data

# AWX API url
awx_url: https://localhost/api/v2

# Check/validate SSL certificates
awx_validate_certs: false

# AWX user + password
awx_user: awx
awx_pass: awx

# List of resources to managed
awx_manage_demo:          true
awx_manage_settings:      true
awx_manage_organizations: true
awx_manage_users:         false
awx_manage_teams:         true
awx_manage_credentials:   true
awx_manage_projects:      true
awx_manage_inventories:   true
awx_manage_hosts:         true
awx_manage_groups:        true
awx_manage_job_templates: true
awx_manage_schedules:     true
awx_manage_roles:         true
awx_delete_hosts:         false
</pre></code>


Example Playbook
----------------

<pre><code>
- name: Converge
  hosts: all
  vars:
  tasks:
    - name: Include role 'ansible-role-awx_config'
      include_role:
        name: ansible-role-awx_config
</pre></code>
