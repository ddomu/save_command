# Playbook:  save_cmd.yml 
==============================================================

Description: 
---------------------------------------------------------------
run multiple commands for NXOS/IOS device

save its result to ./result/<DEVICE_NAME>_<HHMM>.md file 


## Variable 
---------------------------------------------------------------
Change your command list in cmd variable list in save_nxos_cmd.yml 
````yml
  vars:
    cmd:
    - show version
    - show module 
    - show fex
    - show feature
    - show license usage
    - show license host-id
    - show vpc brief
    - show interface status
````
ansible-playbook save_cmd.yml 


## Inside Playbook
----------------
```yml
  #BACKUP 
  roles: 
  - role: backup_config
    vars:
      dest_dir: "./result"
  
  # COMMAND RUNNING 
  tasks: 
  - name: RUN MULTIPLE COMMANDS ON NXOS AND IOS 
    ios_command:
      commands: "{{cmd}}"
    register: output   # output.stdout = ['cmd1','cmd2']

  # SAVE FILES 
  - name: COMMAND RESULT SAVE
    template:
      src: ./template/show_cmd.j2
      dest: ./result/{{inventory_hostname}}_{{timestamp}}.md

```

SOURECE
-------------------
@hjtools
/home/hj/ansible/SWT_upgrade     