# DRAFT - Installing Statellite with Ansible - DRAFT

Update 2021-04-23

## Pre-reqs

- Add these aliases to your bash.rc file at the bottom
            
      # vi ~/.bashrc
      
      alias a='ansible'
      alias ad='ansible-doc'
      alias ag='ansible-galaxy'
      alias ap='ansible-playbook'
      alias av='ansible-vault'

- load aliases
      
      # .  ~/.bashrc

- edit .vimrc and add the following lines.  The shows visually the identation levels which helpful for editing yaml files

      # vi ~/.vimrc

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
      # cd sat-build
      # cp /etc/ansible/ansible.cfg .
      
- Edit ansible.cfg

      # vi ansible.cfg
     
  - Add the following line to the ansible.cfg file

            inventory = ~/src/sat-build/inventory
  
- create an invenotry directory in the sat-buid dir and create an inventory file
  
      # mkdir inventory
      # vi ~/src/etc/inventory

  - Add the following information to the inventory file. 

      [satellite]
      sat01

- You can now test your ansible enginge and connectivity to the target install server for Satellite. Run the command from the ~/sat-build dir

      # ansible Satellite -m ping

- Create a collections directory under the sat-buid dir

      # mkdir collections 
      # cd collectoins

- Create a requirements.yml file to ???

      # vi requirements.yml
      
  - Add the following content to the requirements.yml file

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
   
  - Go back to the sat-build dir
  
            # cd ..
  
- Install the collections

      # ag collection install -r collections/requirements.yml 
      
- Installed collections (roles, variables, modules) in root directory as a system-wide location

      # ls ~/.ansible/collections

In the sat-buid dir create the following file 

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
  
  
  
  
  
  df -hT | egrep -v tempfs
  
  
 ## Notes if you are installing with AWX and not the Red Hat Ansible Automation Platform
 
  Yum module install python36 (may be there alredy)
test python3

