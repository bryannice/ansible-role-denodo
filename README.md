Ansible Role: Denodo Solution Manager
=========

This role installs and configures Denodo Solution Manager.

ToDo's
---------------

[_] Update molecule unit test to provide coverage for RHEL based systems.

[_] Update role to install Denodo Solution Manager on Debian based systems.

[_] Update role to install Denodo Solution Manager on Windows based systems.

[_] Update molecule unit test to provide coverage for Debian based systems.

[_] Update molecule unit test to provide coverage for Windows based systems.

Role Variables
----------------

These are the variables to set by a playbook. The defaults folder contains logic when these variables are not set and the vars folder handles the reconciliation between variables set by a playbook and defaults. To see all default values see defaults/main.

#### Controlling the Installer Behavior

To control the installer behavior, set these variables. Ensure `enabled` is set to true to tell the role to execute the installation process.

```ansible
solution_manager_installer:
  common: 
  enabled:
  denodo_root_directory:
  denodo_installer_path:
  derby_port:
  diagnosticmonitoringtool:
  install_path:
  install_license_manager_server_service: 
  install_solution_manager_server_service: 
  install_vdp_server_service: 
  install_web_apps_services: 
  license_file:
  licensemanager_port:  
  locale: 
  port:  
  server_asyncrmi_port: 
  server_odbc_port: 
  server_shutdownport: 
  server_port: 
  server_factory_port: 
  wc_aux_jmx_port: 
  wc_jmx_port: 
  wc_http_port: 
  wc_shutdown_port:  
```

#### Enabling the SSL and TLS

Ensure `enabled` is set to true to tell the role to execute the SSL/TLS configuration steps.

```ansible
ssl:
  enabled: 
  key_store:
  key_store_password:
  key_store_password_encrypted: 
  trust_store_enabled: 
  trust_store:
  trust_store_password:
  trust_store_password_encrypted: 
  jmxremote_ssl_config_file:  
```

Example Playbook
----------------

Including an example of how to use the role in a playbook using environment variables to set these variables.

```ansible
- name: Apply Ansible playbook
  hosts: all
  become: true
  gather_facts: true
  vars:
    solution_manager_installer:
      enabled: "{{ lookup('env', 'DENODO_SM_INSTALLER_ENABLBED') | default('true', true) }}"
      denodo_root_directory: "{{ lookup('env', 'DENODO_ROOT_DIRECTORY') | default('/opt/denodo', true) }}"
      denodo_installer_path: "{{ lookup('env', 'DENODO_SM_INSTALLER_PATH') | default('/data', true) }}"
      derby_port: "{{ lookup('env', 'DENODO_SM_DERBY_PATH') | default('11526', true) }}"
      install_path: "{{ lookup('env', 'DENODO_SM_INSTALL_PATH') | default('/opt/denodo/solution_manager', true) }}"
      license_file: "{{ lookup('env', 'DENODO_SM_LICENSE_FILE') | default('', true) }}"
      licensemanager_port: "{{ lookup('env', 'DENODO_SM_LICENSEMANAGER_PORT') | default('10091', true) }}"
      locale: "{{ lookup('env', 'DENODO_SM_LOCALE') | default('us_pst', true) }}"
      port: "{{ lookup('env', 'DENODO_SM_PORT') | default('10090', true) }}"
      server_asyncrmi_port: "{{ lookup('env', 'DENODO_SM_SERVER_ASYNCRMI_PORT') | default('19999', true) }}"
      server_odbc_port: "{{ lookup('env', 'DENODO_SM_SERVER_ODBC_PORT') | default('19996', true) }}"
      server_shutdownport: "{{ lookup('env', 'DENODO_SM_SERVER_SHUTDOWNPORT') | default('19998', true) }}"
      server_port: "{{ lookup('env', 'DENODO_SM_SERVER_PORT') | default('19997', true) }}"
      server_factory_port: "{{ lookup('env', 'DENODO_SM_SERVER_FACTORY_PORT') | default('19995', true) }}"
      wc_aux_jmx_port: "{{ lookup('env', 'DENODO_SM_WC_AUX_JMX_PORT') | default('19097', true) }}"
      wc_jmx_port: "{{ lookup('env', 'DENODO_SM_WC_JMX_PORT') | default('19098', true) }}"
      wc_http_port: "{{ lookup('env', 'DENODO_SM_WC_HTTP_PORT') | default('19090', true) }}"
      wc_shutdown_port: "{{ lookup('env', 'DENODO_SM_WC_SHUTDOWN_PORT') | default('19099', true) }}"
    ssl:
      enabled: "{{ lookup('env', 'DENODO_SM_SSL_ENABLBED') | default('false', true) }}"
    denodo_monitor:
      enabled: "{{ lookup('env', 'DENODO_SM_MONITOR_ENABLBED') | default('false', true) }}"
  pre_tasks:
    - name: Gather package facts
      package_facts:
        manager: auto
  tasks:
    - name: Execute Denodo Solution Manager install process
      include_role:
        name: denodo-solution-manager
```

Testing Role
----------------

The ansible molecule container needs to mount the host machine's /var/run/docker.sock to have the ability to spawn another container representing the environment to unit test the role in. Once the environment container is running, then the molecule container will connect to the test environment container to run the test ansible playbook.

![ansible role unit testing](assets/ansible_role_unit_testing.png)

Example docker command to run molecule:

```ansible
docker run --rm -it \
    --env MOLECULE_NO_LOG="false" \
    -v "$(pwd)":/tmp/$(basename "${PWD}"):ro \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v ~/.ssh:/root/.ssh \
    -w /tmp/$(basename "${PWD}") \
    quay.io/ansible/molecule:3.1.5 \
    molecule test
```

License
-------

[GPLv3](LICENSE)

References
----------

- [Yamllint](https://yamllint.readthedocs.io/en/latest/)
- [Molecule Docker Configuration](https://molecule.readthedocs.io/en/2.22/configuration.html#docker)
- [Linux startup scripts for Solution Manager and License Manager](https://community.denodo.com/kb/view/document/Linux%20startup%20scripts%20for%20Solution%20Manager%20and%20License%20Manager?category=Operation)
