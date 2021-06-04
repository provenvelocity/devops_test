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


