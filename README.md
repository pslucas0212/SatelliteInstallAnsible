# SatelliteInstallAnsible

## Pre-reqs

- Add these aliases to your bash.rc file to the bottom
alias a='ansible'
alias ad='ansible-doc'
alias ag='ansible-galaxy'
alias ap='ansible-playbook'
alias av='ansible-vault'
vi ~/.bashrc

- load aliases
      # .  ~/.bashrc
edit .vimrc

- Set vi identation levels
vi ~/.vimrc add these lines

autocmd FileType yaml setlocal ai ts=2 sw=2 et cursorcolumn
autocmd FileType yml setlocal ai ts=2 sw=2 et cursorcolumn


- Generate SSH key for Ansible to login in to the target Satellite server

      # ssh-keygen -t rsa -b 2048
 
   - Copy key to the Satellite server

          # ssh-copy-id root@sat01.example.com
          
- In root home directory

      # mkdir src
      # cd src
      # mkdir sat-build
      # cp /etc/ansible-cf
      # mkdir inventory
      # vi ~/src/etc/inventory

- Add the following information to the inventory file
[satellite]
sat01


Yum module install python36 (may be there alredy)
test python3

from sat-build directory
ansible Satellite -m ping

mkdir collections (under sat-buid dir)

cd collections

vi requirements.yml
---
collections:
  - name: jjaswanson4.setup_rhel_for_satellite
    source: https://galaxy.ansible.com
  - name: jjaswanson4.install_satellite
    source: https://galaxy.ansible.com
  - name: jjaswanson4.configure_satellite
    source: https://galaxy.ansible.com
  - name: ansible.posix
    source: https://galaxy.ansible.com
    
    
  cd .. (back to sat-build dir)
  
  
      # ag collection install -r collections/requirements.yml 
      
- Installed collections (roles, variables, modules) in root directory as a system-wide location

      # ls ~/.ansible/collections

in sat-build dir 

    # vi 01-validate-rhel.yml
    
    
- Add the follwing to the 01-validate-rhel.yml file

---

- name: setup server to be satellite or a capsule
  hosts:
    - satellite
  become: true
  collections:
    - jjaswanson4.setup_rhel_for_satellite
  pre_tasks:
    - name: load in satellite vars
      include_vars:
        file: vars-verify-rhel-sat01.example.com.yml
  roles:
    - jjaswanson4.setup_rhel_for_satellite.validate_rhel

- In the ~/sat-build directory create a vars file
      # vi vars
      
  - Add following content the vars file
---
satellite:
  logical_volumes: []

