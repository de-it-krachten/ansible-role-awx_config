[![CI](https://github.com/de-it-krachten/ansible-role-awx_config/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-awx_config/actions?query=workflow%3ACI)


# ansible-role-awx_config

AWX configuration (configuration-as-code)



## Dependencies

#### Roles
- deitkrachten.awx_cli

#### Collections
- awx.awx

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- Red Hat Enterprise Linux 10<sup>1</sup>
- RockyLinux 8<sup>1</sup>
- RockyLinux 9<sup>1</sup>
- RockyLinux 10<sup>1</sup>
- OracleLinux 8<sup>1</sup>
- OracleLinux 9<sup>1</sup>
- OracleLinux 10<sup>1</sup>
- AlmaLinux 8<sup>1</sup>
- AlmaLinux 9<sup>1</sup>
- AlmaLinux 10<sup>1</sup>
- Debian 11 (Bullseye)<sup>1</sup>
- Debian 12 (Bookworm)<sup>1</sup>
- Debian 13 (Trixie)<sup>1</sup>
- Ubuntu 20.04 LTS<sup>1</sup>
- Ubuntu 22.04 LTS<sup>1</sup>
- Ubuntu 24.04 LTS<sup>1</sup>
- Fedora 42<sup>1</sup>
- Fedora 43<sup>1</sup>

Note:
<sup>1</sup> : no automated testing is performed on these platforms


## Role Variables
### defaults/main.yml
<pre><code>
# List of organization
awx_organizations: []

# path which organizational configs
awx_organization_path: data

# AWX API url
awx_url: https://localhost

# Check/validate SSL certificates
awx_validate_certs: false

# AWX user + password
awx_user: 'admin'
awx_pass: 'Admin123!'

# List of resources to managed
awx_manage_demo: true
awx_manage_settings: true
awx_manage_organizations: true
awx_manage_users: false
awx_manage_teams: true
awx_manage_credentials: true
awx_manage_projects: true
awx_manage_inventories: true
awx_manage_hosts: true
awx_manage_groups: true
awx_manage_job_templates: true
awx_manage_schedules: true
awx_manage_roles: true
awx_delete_hosts: false
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'awx_config'
  hosts: all
  vars:
    awx_url: https://localhost
    awx_validate_certs: false
    awx_user: admin
    awx_pass: Admin123!
    awx_manage_roles: false
    awx_version: 24.6.1
    awx_organizations:
      - name: organization1
        short_name: org1
        description: Company organization nr1
        file: org1.yml
      - name: organization2
        short_name: org2
        description: Company organization nr2
        file: org2.yml
    awx_organization_path: molecule/default/tests/data
  roles:
    - deitkrachten.awx_cli
  tasks:
    - name: Pause play until a URL is reachable from this host
      uri:
        url: '{{ awx_url }}'
        validate_certs: '{{ awx_validate_certs }}'
        follow_redirects: 'yes'
        method: GET
      register: _result
      until: _result.status == 200
      retries: 30
      delay: 10
    - name: Get Oauth token
      include_role:
        name: awx_config
        tasks_from: login.yml
    - name: Include role 'awx_config'
      include_role:
        name: awx_config
</pre></code>
