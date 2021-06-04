# Collections Plugins Directory

python3 -m venv ~/.pv_devops/pv_devops_venv
source ~/.pv_devops/pv_devops_venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt



rm -rf ~/.cache/molecule/
rm -rf ~/.cache/collections/
rm -rf ~/.cache/roles/
rm -rf ~/.cache/ansible-lint/


This directory can be used to ship various plugins inside an Ansible collection. Each plugin is placed in a folder that
is named after the type of plugin it is in. It can also include the `module_utils` and `modules` directory that
would contain module utils and modules respectively.

Here is an example directory of the majority of plugins currently supported by Ansible:

```
└── plugins
    ├── action
    ├── become
    ├── cache
    ├── callback
    ├── cliconf
    ├── connection
    ├── filter
    ├── httpapi
    ├── inventory
    ├── lookup
    ├── module_utils
    ├── modules
    ├── netconf
    ├── shell
    ├── strategy
    ├── terminal
    ├── test
    └── vars
```

A full list of plugin types can be found at [Working With Plugins](https://docs.ansible.com/ansible/2.11/plugins/plugins.html).
