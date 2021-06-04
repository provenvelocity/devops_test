# Ansible Collection - devops_test

Documentation for the collection.


###Setup
python3 -m venv ~/.pv_devops/pv_devops_venv
source ~/.pv_devops/pv_devops_venv/bin/activate
pip install --upgrade pip
pip install -r requirements_withlock.txt


###Clean Environment

I had to do this once because molecule was not changing even when i changed the file it was reporting the old file deleted this and it all worked. 

rm -rf ~/.cache/molecule/
rm -rf ~/.cache/collections/
rm -rf ~/.cache/roles/
rm -rf ~/.cache/ansible-lint/

###to reproduce the import-playbook issue. 

This looks to be an issue with this code... It does not see the playbook in the FQCN

https://github.com/ansible/ansible/blob/2cbfd1e350cbe1ca195d33306b5a9628667ddda8/lib/ansible/playbook/playbook_include.py

```
 molecule converge
```

###to reproduce the Molecule ENV issue 

 

###