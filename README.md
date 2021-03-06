[![CI](https://github.com/de-it-krachten/ansible-role-awx_config/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-awx_config/actions?query=workflow%3ACI)


# ansible-role-awx_config

AWX configuration (configuration-as-code)


Platforms
--------------

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- AlmaLinux 8<sup>1</sup>
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

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
- name: sample playbook for role 'awx_config'
  hosts: all
  vars:

    # olam
    olam_admin_username: admin
    olam_admin_password: admin
    olam_disable_ipv6: true
    olam_demo_data: false

    # awx_config
    awx_url: https://localhost/api/v2
    awx_validate_certs: false
    awx_user: admin
    awx_pass: admin
    awx_manage_roles: false

    awx_organizations:
      - name: organization1
        short_name: org1
        description: Company organization nr1
        file: org1.yml
        #galaxy_credentials:
        #  - Ansible Galaxy
      - name: organization2
        short_name: org2
        description: Company organization nr2
        file: org2.yml

    awx_organization_path: molecule/default/tests/data

  tasks:
    - name: Include role 'olam'
      include_role:
        name: olam

    - name: Flush handlers
      meta: flush_handlers

    - name: Pause play until a URL is reachable from this host
      uri:
        url: "{{ awx_url }}"
        validate_certs: "{{ awx_validate_certs }}"
        follow_redirects: yes
        method: GET
      register: _result
      until: _result.status == 200
      retries: 30
      delay: 10

    - name: Include role 'awx_config'
      include_role:
        name: awx_config
</pre></code>
