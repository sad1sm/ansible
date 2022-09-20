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
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.
* [vector-role](https://github.com/sad1sm/vector-role/tree/v.1.2)