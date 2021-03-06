# ansible-getting-started

## Adhoc configuration with indempotent modules
- Create a ".sh" file
- Make the script executable: chmod +x filename.sh
- Now run the script with "./filename.sh" to repeat configuration
- "git config --add" is not indempotent because it duplicates config
- Shell scripts are imperative (concerned with how a desired stage is reached) whereas ansible is declarative (What desired state should look like)
- Reconciliation is the difference between current and desired state, and the what's necessary to get from the former to the latter.
- Procedural / imperative is partially automated with commands; Declarative is fully automated. 
### Installing Ansible
- pip --version (pip install -U pip, to upgrade)
- pip install ansible
- ansible --version
- ============================================================
- ansible-doc -t connection -l
- ansible + tab
- "ansible" Ad-hoc is meant for running one-off scenarios that aren't really necessary to keep permanently.
- ansible -h
- modules don't have to support idempotence
- ============================================================
- Disseminating a .gitconfig with ansible Ad-hoc and the copy module
- ansible -m copy -a "src=name-of-file dest=specify-destination" localhost
- ansible == Ad-hoc Ansible Command; -m == module-name; -a == args; localhost == managed node/ aka host/ target
- bat ~/.gitconfig
- git config --global --list would show the copied configuration
- ansible -m copy -a "src=name-of-file dest=specify-destination" --check localhost
- --check flag checks if changes would be made. It is a dry run
- ansible -m copy -a "src=name-of-file dest=specify-destination" --diff localhost
- --diff shows what will change or did change.

## Playbooks
### Here's an example script analogy of scripting ansible ad-hoc calls to understand the purpose of playbooks.

- Ensure ~/.gitconfig is basaed on my master.gitconfig
    - ansible -m copy -a "src=../adhoc/master.gitconfig dest=~/.gitconfig" localhost

- Ensure `bat` is installed
  - A `cat` replacement for syntax highlighting!
  - One of the best highlighters
    - ansible -m homebrew -a "name=bat state=latest" localhost

- Ensure `jq` is installed
    - CLI tool for working with json data! 
    - prettify, format, mutate, select subsets, etc
        - ansible -m homebrew -a "name=jq state=latest" localhost

## Configuring multiple hosts
### Spin up Ansible multi VM lab with Vagrant

- Vagrant makes it easy to spin up VMs (in this case using virtualbox and pre-built vagrant boxes for ubuntu and centos)

__Instructions for following along__

- Install
  - Vagrant: <https://www.vagrantup.com/docs/installation/>
  - VirtualBox: <https://www.virtualbox.org/wiki/Downloads>
- Create VMs
  - `cd` into this folder
  - `vagrant up` creates all VMs
    - or `vagrant up ubuntu10` to create one of the VMs
    - or pass other patterns to `vagrant up` to create other subsets of the VMs

### Notes

- As time passes you may want to update the OS versions and respective vagrant boxes in the `Vagrantfile`
  - Or, change vagrant boxes to simulate a different environment (ie OS) and experiment with what you can do with Ansible in that environment.
  - Vagrant box search: <https://app.vagrantup.com/boxes/search>
- Vagrant docs: <https://www.vagrantup.com/docs/>
- Vagrant provisioners for Ansible:
  - Vagrant provides two Ansible provisioners that are alternatives  to the way I demo Ansible in the course. Once you're comfortable with Ansible, these are a nice way to combine the two tools in a way that declaratively describes what Vagrant should have Ansible provision.
  - Run `ansible-playbook` on VM host: <https://www.vagrantup.com/docs/provisioning/ansible.html>
  - Run `ansible-playbook` on guest VM: <https://www.vagrantup.com/docs/provisioning/ansible_local.html>


## Productively learning and using ansible
- Spin up an ansible repl using docker
    - docker run --rm -it python bash
    - ansible-console || ansible-console localhost

## Alternatives to SSH for connecting to hosts
__Docker__
- docker container ps -a
- ansible-playbook create-container.yml || ansible-playbook create-container.yml -v 
-  -v; verbosity, dumps list_all
- ======================================
- ansible-console containers
- git_config list_all=yes scope=global
- docker container exec -it ansible_container_test1 bash
 - git config --global --list
- =======================================
- ansible-playbook cleanup.yml

## Exploiting roles and collections with ansible-galaxy
- ansible-galaxy -h
- __Consume__
    - Find/ Search for a role/collection
    - Install (download)
    - Comprehend & Trust
    - Run contained test in a test environment (docker run --rm -it python bash)
    - Add to playbook
- __Developing/Publishing__
    - Extract
    - Test
    - Publish
    - Mollify