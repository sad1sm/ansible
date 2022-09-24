# Домашнее задание к занятию "08.05 Тестирование Roles"

### Molecule

1. Запустите  `molecule test -s centos7` внутри корневой директории clickhouse-role, посмотрите на вывод команды.

* Не понял в чем смысл указывать команду с указанием сценария которого в роли нет, получил, как и ожидалось, ошибку. Потом запустил со стандартным сценарием, пришлось писать Dockerfile, так как yum не работает в centos 8 из-за проблем с репозиториями. Вывод посмотрел, прикладываю ниже:

```js
$ molecule test

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=instance)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: ~/netology/ansible08/clickhouse-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None) 
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'dockerfile': 'Dockerfile.j2', 'image': 'docker.io/pycontribs/centos:8', 'name': 'instance', 'pre_build_image': False})

TASK [Create Dockerfiles from image names] *************************************
changed: [localhost] => (item={'dockerfile': 'Dockerfile.j2', 'image': 'docker.io/pycontribs/centos:8', 'name': 'instance', 'pre_build_image': False})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'diff': [], 'dest': '~/.cache/molecule/clickhouse-role/default/Dockerfile_docker_io_pycontribs_centos_8', 'src': '~/.ansible/tmp/ansible-tmp-1663615597.603861-11307-106272084984353/source', 'md5sum': 'a1f679cbe200c3fe4dca5935f910af25', 'checksum': '6dd2e88764f996aab2d59debba8fcb474e61ccbc', 'changed': True, 'uid': 1000, 'gid': 1000, 'owner': 'f1tz', 'group': 'f1tz', 'mode': '0600', 'state': 'file', 'size': 250, 'invocation': {'module_args': {'src': '~/.ansible/tmp/ansible-tmp-1663615597.603861-11307-106272084984353/source', 'dest': '~/.cache/molecule/clickhouse-role/default/Dockerfile_docker_io_pycontribs_centos_8', 'mode': '0600', 'follow': False, '_original_basename': 'Dockerfile.j2', 'checksum': '6dd2e88764f996aab2d59debba8fcb474e61ccbc', 'backup': False, 'force': True, 'unsafe_writes': False, 'content': None, 'validate': None, 'directory_mode': None, 'remote_src': None, 'local_follow': None, 'owner': None, 'group': None, 'seuser': None, 'serole': None, 'selevel': None, 'setype': None, 'attributes': None}}, 'failed': False, 'item': {'dockerfile': 'Dockerfile.j2', 'image': 'docker.io/pycontribs/centos:8', 'name': 'instance', 'pre_build_image': False}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
ok: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:8)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'dockerfile': 'Dockerfile.j2', 'image': 'docker.io/pycontribs/centos:8', 'name': 'instance', 'pre_build_image': False})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '473460073557.11985', 'results_file': '~/.ansible_async/473460073557.11985', 'changed': True, 'item': {'dockerfile': 'Dockerfile.j2', 'image': 'docker.io/pycontribs/centos:8', 'name': 'instance', 'pre_build_image': False}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=7    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Include clickhouse-role] *************************************************

TASK [clickhouse-role : Install clickhouse distrib] ****************************
changed: [instance] => (item=clickhouse-common-static)
changed: [instance] => (item=clickhouse-client)
changed: [instance] => (item=clickhouse-server)

TASK [clickhouse-role : Flush handlers] ****************************************

RUNNING HANDLER [clickhouse-role : Start clickhouse service] *******************
changed: [instance]

TASK [clickhouse-role : Create database] ***************************************
changed: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Include clickhouse-role] *************************************************

TASK [clickhouse-role : Install clickhouse distrib] ****************************
ok: [instance] => (item=clickhouse-common-static)
ok: [instance] => (item=clickhouse-client)
ok: [instance] => (item=clickhouse-server)

TASK [clickhouse-role : Flush handlers] ****************************************

TASK [clickhouse-role : Create database] ***************************************
ok: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.

```js
$ molecule init scenario --driver-name docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in ~/netology/ansible08/vector-role/molecule/default successfully.
```

3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

* Протестировал, ошибки исправил, добавил совместимость с Debian.

4. Добавьте несколько assert'ов в verify.yml файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска, etc). Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
```js
PLAY [Verify] ******************************************************************

TASK [stat vector.toml] ********************************************************
ok: [debian]
ok: [centos8]

TASK [check if vector.toml exists] *********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}
ok: [debian] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}

TASK [validate config] *********************************************************
changed: [debian]
changed: [centos8]

TASK [check if vector.toml valid] **********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}
ok: [debian] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian                     : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
5. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.  

* [vector-role](https://github.com/sad1sm/vector-role/tree/v.1.1)

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example)

* Добавил.

2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo - путь до корня репозитория с vector-role на вашей файловой системе.
```
$ docker run --privileged=True -v ~/netology/ansible08/vector-role/:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash
[root@83ed0e65d4c4 vector-role]#
```
3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.

* Посмотрел.  
5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.
6. Пропишите правильную команду в `tox.ini` для того чтобы запускался облегчённый сценарий.
8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
```js
$ docker run --privileged=True -v ~/netology/ansible08/vector-role/:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash
[root@4596d061bac4 vector-role]# tox
py37-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.0.0,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.2,certifi==2022.9.24,cffi==1.15.1,chardet==5.0.0,charset-normalizer==2.1.1,click==8.1.3,click-help-colors==0.9.1,commonmark==0.9.1,cookiecutter==2.1.1,cryptography==38.0.1,distro==1.7.0,enrich==1.2.7,idna==3.4,importlib-metadata==4.12.0,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,lxml==4.9.1,MarkupSafe==2.1.1,molecule==3.4.0,molecule-podman==1.0.1,packaging==21.3,paramiko==2.11.0,pathspec==0.10.1,pluggy==0.13.1,pycparser==2.21,Pygments==2.13.0,PyNaCl==1.5.0,pyparsing==3.0.9,python-dateutil==2.8.2,python-slugify==6.1.2,PyYAML==5.4.1,requests==2.28.1,rich==12.5.1,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.1.0,text-unidecode==1.3,typing_extensions==4.3.0,urllib3==1.26.12,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.8.1
py37-ansible210 run-test-pre: PYTHONHASHSEED='3591171718'
py37-ansible210 run-test: commands[0] | molecule test -s default --destroy always
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
WARNING  Failed to locate command: [Errno 2] No such file or directory: 'git': 'git'
INFO     Guessed /opt/vector-role as project root directory
WARNING  Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /root/.cache/ansible-lint/b984a4/roles/vector-role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/b984a4/roles
INFO     Running default > dependency
INFO     Running ansible-galaxy collection install --force -v containers.podman:>=1.7.0
INFO     Running ansible-galaxy collection install --force -v ansible.posix:>=1.3.0
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '352417015136.94', 'results_file': '/root/.ansible_async/352417015136.94', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /opt/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos8 registry username: None specified") 

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: f1tz/centos8mirrorfix:latest") 

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos8)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=f1tz/centos8mirrorfix:latest) 

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos8 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos8: None specified) 

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (298 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (297 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (296 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (295 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (294 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (293 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (292 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (291 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (290 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (289 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (288 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (287 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (286 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (285 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (284 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (283 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (282 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (281 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (280 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (279 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (278 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (277 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (276 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (275 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (274 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (273 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (272 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (271 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (270 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (269 retries left).
changed: [localhost] => (item=centos8)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
changed: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
changed: [centos8]

TASK [vector-role : Creates directory] *****************************************
changed: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
ok: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
ok: [centos8]

TASK [vector-role : Creates directory] *****************************************
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [stat vector.toml] ********************************************************
ok: [centos8]

TASK [check if vector.toml exists] *********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}

TASK [validate config] *********************************************************
changed: [centos8]

TASK [check if vector.toml valid] **********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '242049887591.3297', 'results_file': '/root/.ansible_async/242049887591.3297', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
py37-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.0.0,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.2,certifi==2022.9.24,cffi==1.15.1,chardet==5.0.0,charset-normalizer==2.1.1,click==8.1.3,click-help-colors==0.9.1,commonmark==0.9.1,cookiecutter==2.1.1,cryptography==38.0.1,distro==1.7.0,enrich==1.2.7,idna==3.4,importlib-metadata==4.12.0,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,lxml==4.9.1,MarkupSafe==2.1.1,molecule==3.4.0,molecule-podman==1.0.1,packaging==21.3,paramiko==2.11.0,pathspec==0.10.1,pluggy==0.13.1,pycparser==2.21,Pygments==2.13.0,PyNaCl==1.5.0,pyparsing==3.0.9,python-dateutil==2.8.2,python-slugify==6.1.2,PyYAML==5.4.1,requests==2.28.1,rich==12.5.1,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.1.0,text-unidecode==1.3,typing_extensions==4.3.0,urllib3==1.26.12,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.8.1
py37-ansible30 run-test-pre: PYTHONHASHSEED='3591171718'
py37-ansible30 run-test: commands[0] | molecule test -s default --destroy always
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
WARNING  Failed to locate command: [Errno 2] No such file or directory: 'git': 'git'
INFO     Guessed /opt/vector-role as project root directory
WARNING  Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /root/.cache/ansible-lint/b984a4/roles/vector-role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/b984a4/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '315670920865.3457', 'results_file': '/root/.ansible_async/315670920865.3457', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /opt/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos8 registry username: None specified") 

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: f1tz/centos8mirrorfix:latest") 

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos8)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=f1tz/centos8mirrorfix:latest) 

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos8 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos8: None specified) 

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item=centos8)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
changed: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
changed: [centos8]

TASK [vector-role : Creates directory] *****************************************
changed: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
ok: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
ok: [centos8]

TASK [vector-role : Creates directory] *****************************************
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [stat vector.toml] ********************************************************
ok: [centos8]

TASK [check if vector.toml exists] *********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}

TASK [validate config] *********************************************************
changed: [centos8]

TASK [check if vector.toml valid] **********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '439532805994.6104', 'results_file': '/root/.ansible_async/439532805994.6104', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
py39-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==2.2.1,ansible-lint==5.1.3,arrow==1.2.3,attrs==22.1.0,bcrypt==4.0.0,binaryornot==0.4.4,bracex==2.3.post1,Cerberus==1.3.2,certifi==2022.9.24,cffi==1.15.1,chardet==5.0.0,charset-normalizer==2.1.1,click==8.1.3,click-help-colors==0.9.1,commonmark==0.9.1,cookiecutter==2.1.1,cryptography==38.0.1,distro==1.7.0,enrich==1.2.7,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.16.0,lxml==4.9.1,MarkupSafe==2.1.1,molecule==3.4.0,molecule-podman==1.0.1,packaging==21.3,paramiko==2.11.0,pathspec==0.10.1,pluggy==0.13.1,pycparser==2.21,Pygments==2.13.0,PyNaCl==1.5.0,pyparsing==3.0.9,pyrsistent==0.18.1,python-dateutil==2.8.2,python-slugify==6.1.2,PyYAML==5.4.1,requests==2.28.1,rich==12.5.1,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.1.0,text-unidecode==1.3,urllib3==1.26.12,wcmatch==8.4.1,yamllint==1.26.3
py39-ansible210 run-test-pre: PYTHONHASHSEED='3591171718'
py39-ansible210 run-test: commands[0] | molecule test -s default --destroy always
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
WARNING  Failed to locate command: [Errno 2] No such file or directory: 'git'
INFO     Guessed /opt/vector-role as project root directory
WARNING  Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /root/.cache/ansible-lint/b984a4/roles/vector-role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/b984a4/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '190838247424.6238', 'results_file': '/root/.ansible_async/190838247424.6238', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /opt/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos8 registry username: None specified") 

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: f1tz/centos8mirrorfix:latest") 

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos8)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=f1tz/centos8mirrorfix:latest) 

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos8 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos8: None specified) 

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item=centos8)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
changed: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
changed: [centos8]

TASK [vector-role : Creates directory] *****************************************
changed: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
ok: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
ok: [centos8]

TASK [vector-role : Creates directory] *****************************************
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [stat vector.toml] ********************************************************
ok: [centos8]

TASK [check if vector.toml exists] *********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}

TASK [validate config] *********************************************************
changed: [centos8]

TASK [check if vector.toml valid] **********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '838516937551.8822', 'results_file': '/root/.ansible_async/838516937551.8822', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==2.2.1,ansible-lint==5.1.3,arrow==1.2.3,attrs==22.1.0,bcrypt==4.0.0,binaryornot==0.4.4,bracex==2.3.post1,Cerberus==1.3.2,certifi==2022.9.24,cffi==1.15.1,chardet==5.0.0,charset-normalizer==2.1.1,click==8.1.3,click-help-colors==0.9.1,commonmark==0.9.1,cookiecutter==2.1.1,cryptography==38.0.1,distro==1.7.0,enrich==1.2.7,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.16.0,lxml==4.9.1,MarkupSafe==2.1.1,molecule==3.4.0,molecule-podman==1.0.1,packaging==21.3,paramiko==2.11.0,pathspec==0.10.1,pluggy==0.13.1,pycparser==2.21,Pygments==2.13.0,PyNaCl==1.5.0,pyparsing==3.0.9,pyrsistent==0.18.1,python-dateutil==2.8.2,python-slugify==6.1.2,PyYAML==5.4.1,requests==2.28.1,rich==12.5.1,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.1.0,text-unidecode==1.3,urllib3==1.26.12,wcmatch==8.4.1,yamllint==1.26.3
py39-ansible30 run-test-pre: PYTHONHASHSEED='3591171718'
py39-ansible30 run-test: commands[0] | molecule test -s default --destroy always
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
WARNING  Failed to locate command: [Errno 2] No such file or directory: 'git'
INFO     Guessed /opt/vector-role as project root directory
WARNING  Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /root/.cache/ansible-lint/b984a4/roles/vector-role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/b984a4/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '514152168643.8945', 'results_file': '/root/.ansible_async/514152168643.8945', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /opt/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos8 registry username: None specified") 

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: f1tz/centos8mirrorfix:latest") 

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos8)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=f1tz/centos8mirrorfix:latest) 

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos8 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos8: None specified) 

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item=centos8)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
changed: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
changed: [centos8]

TASK [vector-role : Creates directory] *****************************************
changed: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Install Vector package] ************************************
ok: [centos8]

TASK [vector-role : Install prerequisites for Ansible to install .deb via apt module] ***
skipping: [centos8]

TASK [vector-role : Install Vector package] ************************************
skipping: [centos8]

TASK [vector-role : Configure Vector] ******************************************
ok: [centos8]

TASK [vector-role : Creates directory] *****************************************
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [stat vector.toml] ********************************************************
ok: [centos8]

TASK [check if vector.toml exists] *********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "/etc/vector/vector.toml exists"
}

TASK [validate config] *********************************************************
changed: [centos8]

TASK [check if vector.toml valid] **********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "validation /etc/vector/vector.toml success"
}

PLAY RECAP *********************************************************************
centos8                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '958430373507.11553', 'results_file': '/root/.ansible_async/958430373507.11553', 'changed': True, 'failed': False, 'item': {'image': 'f1tz/centos8mirrorfix:latest', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
_______________________________________________________________________________________________________ summary ________________________________________________________________________________________________________
  py37-ansible210: commands succeeded
  py37-ansible30: commands succeeded
  py39-ansible210: commands succeeded
  py39-ansible30: commands succeeded
  congratulations :)
```
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.
* Комментарий к доработке:  
Провел доработку, действительно по какой-то причине не закоммитил module-podman в tox-requirements.txt.
Так же пришлось собрать образ centos 8 с python и фиксом dnf, т.к. tox капризничал и не хотел собирать из Dockerfile.  
Так же пришлось удалить строку `{% if item.hostname is defined %}--hostname={{ item.hostname }}{% elif item.name is defined %}--hostname={{ item.name }}{% endif %}` из `/opt/vector-role/.tox/py{37,39}-ansible{210,30}/lib/python3.{7,9}/site-packages/molecule_podman/playbooks/create.yml` так как podman на отрез отказывался запускать контейнер с ошибкой `Error: invalid config provided: cannot set hostname when running in the host UTS namespace: invalid configuration`.  
* [vector-role](https://github.com/sad1sm/vector-role/tree/v.1.3)