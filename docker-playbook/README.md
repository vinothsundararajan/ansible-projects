---
    [root@docker-master docker-playbook]# ansible-playbook playbook-docker.yml -i /opt/ansible/inventory/inventory.txt

    PLAY [Do Ansible and check the docker status] *************************************************************************************************************************

    TASK [Gathering Facts] ************************************************************************************************************************************************
    ok: [localhost]

    TASK [Check the docker status to ensure docker is running] ************************************************************************************************************
    ok: [localhost]

    TASK [Print list of images available on remote host] ******************************************************************************************************************
    changed: [localhost]

    TASK [Start the docker container] *************************************************************************************************************************************
    fatal: [localhost]: FAILED! => {"changed": false, "msg": "Failed to import the required Python library (Docker SDK for Python: docker (Python >= 2.7) or docker-py (Python 2.6)) on docker-master.local's Python /usr/bin/python. Please read module documentation and install in the appropriate location. If the required library is installed, but Ansible is using the wrong Python interpreter, please consult the documentation on ansible_python_interpreter, for example via `pip install docker` or `pip install docker-py` (Python 2.6). The error was: No module named docker"}

    PLAY RECAP ************************************************************************************************************************************************************
    localhost                  : ok=3    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0


---

      [root@docker-master docker-playbook]# pip install docker-py
      DEPRECATION: Python 2.7 reached the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 is no longer maintained. pip 21.0 will drop support for Python 2.7 in January 2021. More details about Python 2 support in pip can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support pip 21.0 will remove support for this functionality.
      Collecting docker-py
        Downloading docker_py-1.10.6-py2.py3-none-any.whl (50 kB)
           |████████████████████████████████| 50 kB 25 kB/s
      Collecting docker-pycreds>=0.2.1
        Downloading docker_pycreds-0.4.0-py2.py3-none-any.whl (9.0 kB)
      Collecting websocket-client>=0.32.0
        Downloading websocket_client-0.59.0-py2.py3-none-any.whl (67 kB)
           |████████████████████████████████| 67 kB 632 kB/s
      Requirement already satisfied: requests!=2.11.0,>=2.5.2 in /usr/lib/python2.7/site-packages (from docker-py) (2.26.0)
      Requirement already satisfied: six>=1.4.0 in /usr/lib/python2.7/site-packages (from docker-py) (1.9.0)
      Requirement already satisfied: backports.ssl-match-hostname>=3.5; python_version < "3.5" in /usr/lib/python2.7/site-packages (from docker-py) (3.5.0.1)
      Requirement already satisfied: ipaddress>=1.0.16; python_version < "3.3" in /usr/lib/python2.7/site-packages (from docker-py) (1.0.16)
      Requirement already satisfied: certifi>=2017.4.17 in /usr/lib/python2.7/site-packages (from requests!=2.11.0,>=2.5.2->docker-py) (2021.5.30)
      Requirement already satisfied: urllib3<1.27,>=1.21.1 in /usr/lib/python2.7/site-packages (from requests!=2.11.0,>=2.5.2->docker-py) (1.26.6)
      Requirement already satisfied: idna<3,>=2.5; python_version < "3" in /usr/lib/python2.7/site-packages (from requests!=2.11.0,>=2.5.2->docker-py) (2.10)
      Requirement already satisfied: chardet<5,>=3.0.2; python_version < "3" in /usr/lib/python2.7/site-packages (from requests!=2.11.0,>=2.5.2->docker-py) (4.0.0)
      Installing collected packages: docker-pycreds, websocket-client, docker-py
      Successfully installed docker-py-1.10.6 docker-pycreds-0.4.0 websocket-client-0.59.0
      
      
  ---
      [root@docker-master docker-playbook]# ansible-playbook playbook-docker.yml -i /opt/ansible/inventory/inventory.txt

      PLAY [Do Ansible and check the docker status] *************************************************************************************************************************

      TASK [Gathering Facts] ************************************************************************************************************************************************
      ok: [localhost]

      TASK [Check the docker status to ensure docker is running] ************************************************************************************************************
      ok: [localhost]

      TASK [Print list of images available on remote host] ******************************************************************************************************************
      changed: [localhost]

      TASK [Start the docker container] *************************************************************************************************************************************
      changed: [localhost]

      PLAY RECAP ************************************************************************************************************************************************************
      localhost                  : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

---
        [root@docker-master docker-playbook]# docker ps
        CONTAINER ID   IMAGE               COMMAND                  CREATED        STATUS         PORTS                                   NAMES
        077c521c35d4   nginx:1.13-alpine   "nginx -g 'daemon of…"   42 hours ago   Up 7 seconds   0.0.0.0:8000->80/tcp, :::8000->80/tcp   nginx_test1
---

      [root@docker-master docker-playbook]# cat playbook-docker.yml
      -
        name: 'Do Ansible and check the docker status'
        hosts: 'localhost'
        vars:
          docker_diskchk: 'df -hT'
        tasks:
          -
            name: "Check the docker status to ensure docker is running"
            service:
              name: docker
              state: started
          -
            name: "Print list of images available on remote host"
            command: docker image ls
          -
            name: "Start the docker container"
            docker_container:
              name: nginx_test1
              state: started


