# ansible-getting-started

__Adhoc configuration with indempotent modules__
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

__Playbooks__
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
