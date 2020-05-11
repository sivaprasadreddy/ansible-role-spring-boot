# ansible-role-spring-boot
Ansible role for deploying SpringBoot application

## How to use?

**playbook.yml**

```yaml
---
- hosts: localhost
  become: true
  roles:
    - role: https://github.com/sivaprasadreddy/ansible-role-spring-boot.git
      jar_path: /code/artifacts/spring-boot-todolist-0.0.1.jar
      app_name: "todolist"
      app_description: "todo list app"
      app_user: "vagrant"
      app_user_group: "vagrant"
      app_dest_dir: "/var/myapps"
      app_env_vars:
        - name: SPRING_PID_FILE
          value: "/var/myapps/todolist/pid.txt"
        - name: LOGGING_FILE_PATH
          value: "/var/myapps/todolist/todolist.log"
  tasks:
    - name: get service facts
      service_facts:

    - debug:
        var: ansible_facts.services["todolist.service"]

    - name: Check if todolist service is running
      assert:
        that:
          - ansible_facts.services["todolist.service"].state == 'running'
        fail_msg: "Todolist service is not running"
        success_msg: "Todolist service is running"
```

You can try out the role with Vagrant as follows:

**Vagrantfile**

```
Vagrant.configure("2") do |config|
    config.vm.box = "sivalabs/ubuntu_bionic64_java"
    # Run Ansible from the Vagrant VM
    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end
end
```
