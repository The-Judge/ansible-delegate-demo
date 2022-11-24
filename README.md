See https://github.com/ansible/ansible/issues/79450 for details on this repo.

----

    ansible-delegate-demo $ ansible-playbook playbooks/test.yml -vv
    ansible-playbook [core 2.13.5]
      config file = /Users/mrichter/Desktop/ansible-delegate-demo/ansible.cfg
      configured module search path = ['/Users/mrichter/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/local/Cellar/ansible/6.5.0/libexec/lib/python3.10/site-packages/ansible
      ansible collection location = /Users/mrichter/.ansible/collections:/usr/share/ansible/collections
      executable location = /usr/local/bin/ansible-playbook
      python version = 3.10.8 (main, Oct 13 2022, 10:17:43) [Clang 14.0.0 (clang-1400.0.29.102)]
      jinja version = 3.1.2
      libyaml = True
    Using /Users/mrichter/Desktop/ansible-delegate-demo/ansible.cfg as config file
    Skipping callback 'default', as we already have a stdout callback.
    Skipping callback 'minimal', as we already have a stdout callback.
    Skipping callback 'oneline', as we already have a stdout callback.
    
    PLAYBOOK: test.yml *********************************************************************************************************************************************************************************************************************
    1 plays in playbooks/test.yml
    
    PLAY [Test Playbook] *******************************************************************************************************************************************************************************************************************
    
    TASK [Gathering Facts] *****************************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/playbooks/test.yml:2
    ok: [sbsdevcore01]
    ok: [sbsdevcore02]
    ok: [sbsdevcore03]
    META: ran handlers
    
    TASK [test : debug] ********************************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/main.yml:2
    ok: [sbsdevcore01] => {
        "msg": "my_group_name: sbsdevcore"
    }
    ok: [sbsdevcore02] => {
        "msg": "my_group_name: sbsdevcore"
    }
    ok: [sbsdevcore03] => {
        "msg": "my_group_name: sbsdevcore"
    }
    
    TASK [test : debug] ********************************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/main.yml:5
    ok: [sbsdevcore01] => {
        "msg": [
            "sbsdevcore01",
            "sbsdevcore02",
            "sbsdevcore03"
        ]
    }
    ok: [sbsdevcore02] => {
        "msg": [
            "sbsdevcore01",
            "sbsdevcore02",
            "sbsdevcore03"
        ]
    }
    ok: [sbsdevcore03] => {
        "msg": [
            "sbsdevcore01",
            "sbsdevcore02",
            "sbsdevcore03"
        ]
    }
    
    TASK [test : get random element from groups[my_group_name]] ****************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/main.yml:8
    included: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/get_random_element.yml for sbsdevcore01, sbsdevcore02, sbsdevcore03
    
    TASK [test : take 'list' and return a single element from it 'elem'] *******************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/get_random_element.yml:2
    ok: [sbsdevcore01] => {"ansible_facts": {"elem": "sbsdevcore03"}, "changed": false}
    ok: [sbsdevcore02] => {"ansible_facts": {"elem": "sbsdevcore02"}, "changed": false}
    ok: [sbsdevcore03] => {"ansible_facts": {"elem": "sbsdevcore01"}, "changed": false}
    
    TASK [test : debug] ********************************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/main.yml:13
    ok: [sbsdevcore01] => {
        "msg": "sbsdevcore03"
    }
    ok: [sbsdevcore02] => {
        "msg": "sbsdevcore02"
    }
    ok: [sbsdevcore03] => {
        "msg": "sbsdevcore01"
    }
    
    TASK [test : Main Level Task] **********************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/main.yml:16
    included: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/sub_task.yml for sbsdevcore01, sbsdevcore02, sbsdevcore03
    
    TASK [test : debug] ********************************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/sub_task.yml:2
    ok: [sbsdevcore01] => {
        "msg": "my_execution_node: sbsdevcore03"
    }
    ok: [sbsdevcore02] => {
        "msg": "my_execution_node: sbsdevcore02"
    }
    ok: [sbsdevcore03] => {
        "msg": "my_execution_node: sbsdevcore03"
    }
    
    TASK [test : This is my sub-task] ******************************************************************************************************************************************************************************************************
    task path: /Users/mrichter/Desktop/ansible-delegate-demo/roles/test/tasks/sub_task.yml:5
    fatal: [sbsdevcore01 -> sbsdevcore03]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: Could not resolve hostname inventory_hostname: nodename nor servname provided, or not known", "unreachable": true}
    
    NO MORE HOSTS LEFT *********************************************************************************************************************************************************************************************************************
    
    PLAY RECAP *****************************************************************************************************************************************************************************************************************************
    sbsdevcore01               : ok=8    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0   
    sbsdevcore02               : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    sbsdevcore03               : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    
    (base) [last: 4s] [exit: 2] mrichter 08:54:27 (CET) Thu 2022-11-24
    /Users/mrichter/Desktop/ansible-delegate-demo
    ansible-delegate-demo $