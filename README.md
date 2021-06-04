# Ansible Collection - devops_test

Issues in molecule 

https://github.com/ansible-community/molecule/discussions/3140

### Setup
```
python3 -m venv ~/.pv_devops/pv_devops_venv
source ~/.pv_devops/pv_devops_venv/bin/activate
pip install --upgrade pip
pip install -r requirements_withlock.txt
```

### Clean Environment

I had to do this multible time because molecule was not changing even when i changed the file it was reporting the old file deleted this and it all worked.  Have not found how to reproduce
```
rm -rf ~/.cache/molecule/
rm -rf ~/.cache/collections/
rm -rf ~/.cache/roles/
rm -rf ~/.cache/ansible-lint/
```
### To reproduce the import-playbook issue. 

This looks to be an issue with this code... It does not see the playbook in the FQCN

https://github.com/ansible/ansible/blob/2cbfd1e350cbe1ca195d33306b5a9628667ddda8/lib/ansible/playbook/playbook_include.py

```
 molecule converge
```

### To reproduce the Molecule ENV issue 

branch: env_issue 

Simply run the molecule test in debug and you will see that each of the Environments in deploy lint provision all have different ANSIBLE_COLLECTIONS_PATH setting even if i try to override either in the .env or in the molecule file or even in the user level env.

This Needs a new issue opened up but since they keep getting closed I have not opened it yet.

```
 molecule --debug test
```

### Role Issue 

branch: role_folder_issue 

again simply run the molecule test and you will see that the .cache folder is made in the molecule default folder but all other ./ refernces are in the root of the project where the commadn is run. This shows that the role is put in the wrong location in the ticket I outline the code at fault. 
 
https://github.com/ansible-community/molecule/issues/3131

```
 molecule test
```

### Resouces Folder not working on all steps. (Sharing Across Scenarios_)

branch: resource_folder_issue

The documentation states that the Scenarios should be able to share resources using this folder structure. This does not work for converge the provision code is missing the code.Again this comes down to much of the code is duplicated across Scenarios and should be moved to base to have a common library to fork with. 

```
 molecule test
```


### More exsample to come
I have other issue but intl we get a working relation ship and my bugs are not closed im not going to outline them yet in this repo. 
