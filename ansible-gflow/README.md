# ansible-gflow
**G**ateway **F**ai**L** **O**ver **W**an (formely known as Dead Gateway Detection).

- Gflow is a tool for managing multiple gateways on a router.
- Ansible-Gflow is a deployment tool to install gflow on multiple platform.

When launching periodically gflow, it checks if all configured gateways are alive. Depending of *multipath* option value, only the first of all available gateway(s) is(are) used as default gateway.

## Installation

- Install gflow on the hosts you need: `ansible-playbook playbooks/gateway.yml -l gateways -C`
- See [Installation instructions](files/doc/INSTALL.md).
- You could manage **gflow-foobar.ini** from ansible if you want, by adding your configuration file in the template directory.


## Role Variables

| name             | Type             | Description                                                                                                     |
| ---------------- | ---------------- | --------------------------------------------------------------------------------------------------------------- |
| ansible_hostname | Built-in ansible | variable used in [task install-specific](tasks/install-specific.yml) to select your specific gflow config file. |
| gflow_remove     | Bool             | Used to install or remove gflow. [Default](defaults/main.yml):False                                             |


## Example playbook

**gateway.yml**
```yaml
- name: Define Gflow
  hosts: gateways
  roles:
        - role: gflow
```

## Configuration
See: `man gflow` when gflow is installed or [files/doc/README.md](files/doc/README.md)


## License

 - GPLv3 : https://gnu.org/licenses/gpl-3.0.en.html

## Author Information

 - Félix Defrance <gflow@d2france.fr>
