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
###to reproduce the Molecule Collections issue 

branch: Collections_issue 

####Run:

ansible-playbook ./molecule/default/converge.yml -i localhost

####Wrong Result:
```
└─▪ (pv_devops_venv) ansible-playbook ./molecule/default/converge.yml -i localhost
[WARNING]: Unable to parse /Users/H217689/kp/ansible/provenvelocity/devops_test/localhost as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
ERROR! Unable to retrieve file contents
Could not find or access '/Users/H217689/kp/ansible/provenvelocity/devops_test/molecule/default/provenvelocity.devops_test.palybooks.main.yml' on the Ansible Controller.
If you are using a module and expect the file to exist on the remote, see the remote_src option
```
####Also:

Running it like this allows you to see (some times)(some times not) the types ansible views the files as. You can see that only some are playbooks and some are not even though they are. Also it thinks this is a file but it is not there. "molecule/provenvelocity.devops_test.main.yml"... These are all other issues but first im tackling the collections issue.

Ansible lint uses ansible core for this so this is still al ansible issue BTW. ( I think :))

```
─▪ (pv_devops_venv) ansible-lint -vvv
DEBUG    Logging initialized to level 10
DEBUG    Options: Namespace(cache_dir='/Users/H217689/.cache/ansible-lint/5e737a', colored=True, configured=True, cwd=PosixPath('/Users/H217689/kp/ansible/provenvelocity/devops_test'), display_relative_path=True, exclude_paths=['/Users/H217689/kp/ansible/provenvelocity/devops_test/.cache', '/Users/H217689/kp/ansible/provenvelocity/devops_test/.github'], lintables=[], listrules=False, listtags=False, parseable=False, parseable_severity=False, quiet=False, rulesdirs=['/Users/H217689/.pv_devops/pv_devops_venv/lib/python3.9/site-packages/ansiblelint/rules'], skip_list=['204', 'yaml'], tags=[], verbosity=3, warn_list=['experimental', 'role-name'], kinds=[{'jinja2': '**/*.j2'}, {'jinja2': '**/*.j2.*'}, {'requirements': '**/meta/requirements.yml'}, {'galaxy': '**/galaxy.yml'}, {'reno': '**/releasenotes/*/*.{yaml,yml}'}, {'playbook': '**/playbooks/*.{yml,yaml}'}, {'playbook': '**/*playbook*.{yml,yaml}'}, {'role': '**/roles/*/'}, {'tasks': '**/tasks/**/*.{yaml,yml}'}, {'handlers': '**/handlers/*.{yaml,yml}'}, {'vars': '**/{host_vars,group_vars,vars,defaults}/**/*.{yaml,yml}'}, {'meta': '**/meta/main.{yaml,yml}'}, {'yaml': '.config/molecule/config.{yaml,yml}'}, {'requirements': '**/molecule/*/{collections,requirements}.{yaml,yml}'}, {'yaml': '**/molecule/*/{base,molecule}.{yaml,yml}'}, {'requirements': '**/requirements.yml'}, {'playbook': '**/molecule/*/*.{yaml,yml}'}, {'yaml': '**/{.ansible-lint,.yamllint}'}, {'yaml': '**/*.{yaml,yml}'}, {'yaml': '**/.*.{yaml,yml}'}], mock_modules=[], mock_roles=[], loop_var_prefix=None, var_naming_pattern='^[a-z_][a-z0-9_]*$', offline=False, project_dir='.', extra_vars=None, enable_list=[], skip_action_validation=True, rules={}, format='rich', progressive=False, rulesdir=[], use_default_rules=True, config_file='/Users/H217689/kp/ansible/provenvelocity/devops_test/.ansible-lint', version=False)
INFO     Running ansible-galaxy role install --roles-path /Users/H217689/.cache/ansible-lint/5e737a/roles -vr requirements.yml
INFO     Running ansible-galaxy collection install -p /Users/H217689/.cache/ansible-lint/5e737a/collections -vr requirements.yml
INFO     Added ANSIBLE_COLLECTIONS_PATH=/Users/H217689/.cache/ansible-lint/5e737a/collections
DEBUG    Loading rules from /Users/H217689/.pv_devops/pv_devops_venv/lib/python3.9/site-packages/ansiblelint/rules
INFO     Running ansible-galaxy role install --roles-path /Users/H217689/.cache/ansible-lint/5e737a/roles -vr requirements.yml
INFO     Running ansible-galaxy collection install -p /Users/H217689/.cache/ansible-lint/5e737a/collections -vr requirements.yml
INFO     Added ANSIBLE_COLLECTIONS_PATH=/Users/H217689/.cache/ansible-lint/5e737a/collections:/Users/H217689/.cache/ansible-lint/5e737a/collections
Loading custom .yamllint config file, this extends our internal yamllint config.
INFO     Discovered files to lint using: git ls-files -z
INFO     Executing syntax check on molecule/docker/verify.yml (0.83s)
INFO     Executing syntax check on molecule/default/verify.yml (0.84s)
INFO     Executing syntax check on molecule/docker/converge.yml (0.86s)
INFO     Executing syntax check on molecule/default/converge.yml (0.88s)
INFO     Executing syntax check on playbooks/main.yml (0.88s)
DEBUG    Examining tests/test.yml of type yaml
DEBUG    Examining tests/shared_tests.yml of type yaml
DEBUG    Examining molecule/default/converge.yml of type playbook
DEBUG    Examining molecule/docker/verify.yml of type playbook
DEBUG    Examining requirements.yml of type requirements
DEBUG    Examining .yamllint of type yaml
DEBUG    Examining .env.yml of type yaml
DEBUG    Examining molecule/docker/converge.yml of type playbook
DEBUG    Examining molecule/default/molecule.yml of type yaml
DEBUG    Examining playbooks/main.yml of type playbook
DEBUG    Examining molecule/default/verify.yml of type playbook
DEBUG    Examining main.yml of type yaml
DEBUG    Examining galaxy.yml of type galaxy
DEBUG    Examining molecule/docker/molecule.yml of type yaml
DEBUG    Examining .ansible-lint of type yaml
DEBUG    Examining molecule/provenvelocity.devops_test.main.yml of type playbook
DEBUG    Examining tests/shared_tests.yml of type playbook
WARNING  Listing 1 violation(s) that are fatal
load-failure: [Errno 2] No such file or directory: 'molecule/provenvelocity.devops_test.main.yml' (filenotfounderror)
molecule/provenvelocity.devops_test.main.yml:1
```
####Also:

If you notice in the exsample above it does not determen that main.yml in root of the collection is a playbook so it marks it yaml type I then use the same file in the playbooks folder and than get the error below which is the same as the first error and explained below. 

Ansible lint uses ansible core for this so this is still al ansible issue BTW. ( I think :))

```
└─▪ (pv_devops_venv) ansible-lint -vvv
DEBUG    Logging initialized to level 10
DEBUG    Options: Namespace(cache_dir='/Users/H217689/.cache/ansible-lint/5e737a', colored=True, configured=True, cwd=PosixPath('/Users/H217689/kp/ansible/provenvelocity/devops_test'), display_relative_path=True, exclude_paths=['/Users/H217689/kp/ansible/provenvelocity/devops_test/.cache', '/Users/H217689/kp/ansible/provenvelocity/devops_test/.github'], lintables=[], listrules=False, listtags=False, parseable=False, parseable_severity=False, quiet=False, rulesdirs=['/Users/H217689/.pv_devops/pv_devops_venv/lib/python3.9/site-packages/ansiblelint/rules'], skip_list=['204', 'yaml'], tags=[], verbosity=3, warn_list=['experimental', 'role-name'], kinds=[{'jinja2': '**/*.j2'}, {'jinja2': '**/*.j2.*'}, {'requirements': '**/meta/requirements.yml'}, {'galaxy': '**/galaxy.yml'}, {'reno': '**/releasenotes/*/*.{yaml,yml}'}, {'playbook': '**/playbooks/*.{yml,yaml}'}, {'playbook': '**/*playbook*.{yml,yaml}'}, {'role': '**/roles/*/'}, {'tasks': '**/tasks/**/*.{yaml,yml}'}, {'handlers': '**/handlers/*.{yaml,yml}'}, {'vars': '**/{host_vars,group_vars,vars,defaults}/**/*.{yaml,yml}'}, {'meta': '**/meta/main.{yaml,yml}'}, {'yaml': '.config/molecule/config.{yaml,yml}'}, {'requirements': '**/molecule/*/{collections,requirements}.{yaml,yml}'}, {'yaml': '**/molecule/*/{base,molecule}.{yaml,yml}'}, {'requirements': '**/requirements.yml'}, {'playbook': '**/molecule/*/*.{yaml,yml}'}, {'yaml': '**/{.ansible-lint,.yamllint}'}, {'yaml': '**/*.{yaml,yml}'}, {'yaml': '**/.*.{yaml,yml}'}], mock_modules=[], mock_roles=[], loop_var_prefix=None, var_naming_pattern='^[a-z_][a-z0-9_]*$', offline=False, project_dir='.', extra_vars=None, enable_list=[], skip_action_validation=True, rules={}, format='rich', progressive=False, rulesdir=[], use_default_rules=True, config_file='/Users/H217689/kp/ansible/provenvelocity/devops_test/.ansible-lint', version=False)
INFO     Running ansible-galaxy role install --roles-path /Users/H217689/.cache/ansible-lint/5e737a/roles -vr requirements.yml
INFO     Running ansible-galaxy collection install -p /Users/H217689/.cache/ansible-lint/5e737a/collections -vr requirements.yml
INFO     Added ANSIBLE_COLLECTIONS_PATH=/Users/H217689/.cache/ansible-lint/5e737a/collections
DEBUG    Loading rules from /Users/H217689/.pv_devops/pv_devops_venv/lib/python3.9/site-packages/ansiblelint/rules
INFO     Running ansible-galaxy role install --roles-path /Users/H217689/.cache/ansible-lint/5e737a/roles -vr requirements.yml
INFO     Running ansible-galaxy collection install -p /Users/H217689/.cache/ansible-lint/5e737a/collections -vr requirements.yml
INFO     Added ANSIBLE_COLLECTIONS_PATH=/Users/H217689/.cache/ansible-lint/5e737a/collections:/Users/H217689/.cache/ansible-lint/5e737a/collections
Loading custom .yamllint config file, this extends our internal yamllint config.
INFO     Discovered files to lint using: git ls-files -z
INFO     Executing syntax check on molecule/default/converge.yml (0.70s)
INFO     Executing syntax check on molecule/docker/verify.yml (0.82s)
INFO     Executing syntax check on molecule/default/verify.yml (0.83s)
INFO     Executing syntax check on playbooks/main.yml (0.86s)
INFO     Executing syntax check on molecule/docker/converge.yml (0.86s)
WARNING  Listing 1 violation(s) that are fatal
internal-error: Unexpected error code 1 from execution of: ansible-playbook --syntax-check molecule/default/converge.yml
molecule/default/converge.yml:1 [WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that
the implicit localhost does not match 'all'
ERROR! Unable to retrieve file contents
Could not find or access '/Users/H217689/kp/ansible/provenvelocity/devops_test/molecule/default/provenvelocity.devops_test.playbooks.main.yml' on the Ansible Controller.
If you are using a module and expect the file to exist on the remote, see the remote_src option
```

####Expected Result:
Converge and install playbook.

####Issues:
- Ansible does not see current collection as a collection unless it is in there collections folders. Currently ansible lint does not place them there so this command fails. With role_paths you add ":role" to the paths you need to add the current folder if it has a galaxy.yml file in it. So you do not need to copy the current collection outside of the path it is in to see it in the collections struct.
- If you run this with ansible lint you will see that it sets a collections path and copies the collections there. This gets you past one issue but then you get another set of issues. Not all Types are respected and when it runs internaly ansible plaubook command you get back to the core issue which is ansible does not see current collection and in this exsample is not respecting the ANSIBLE_COLLECTION_PATH when run in a nested way auscha s out of ansible -lint. (This may actualy be an issue with ansible lint not passing the configure ENV to the command it runs to test the code. )
- This also happens in molecule becasue it runs the code from ansible-lint.




###to reproduce the Molecule ENV issue 

branch: env_issue 

Simply run the molecule test in debug and you will see that each of the Environments in deploy lint provision all have different ANSIBLE_COLLECTIONS_PATH setting even if i try to override either in the .env or in the molecule file or even in the user level env.

This Needs a new issue opened up but since they keep getting closed I have not opened it yet.


###Role Issue 

branch: role_folder_issue 

again simply run the molecule test and you will see that the .cache folder is made in the molecule default folder but all other ./ refernces are in the root of the project where the commadn is run. This shows that the role is put in the wrong location in the ticket I outline the code at fault. 
 
https://github.com/ansible-community/molecule/issues/3131



