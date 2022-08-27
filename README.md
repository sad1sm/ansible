# Домашнее задание к занятию "08.02 Работа с Playbook"

1. Приготовьте свой собственный inventory файл `prod.yml`.
```
---
clickhouse:
  hosts:
    clickhouse-01:
      ansible_host: 172.24.112.1
      ansible_port: 2222
      ansible_user: ansible
```
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev).
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, установить vector.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
```
$ ansible-lint site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: site.yml
```
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
```
$ ansible-playbook -i inventory/ site.yml --private-key=id_rsa --check

PLAY [Install Clickhouse] **********************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ******************************************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] *************************************************************************************************************
changed: [clickhouse-01]

TASK [Create database] *************************************************************************************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [clickhouse-01]

TASK [Get vector distrib] **********************************************************************************************************************
ok: [clickhouse-01]

TASK [Install Vector packages] *****************************************************************************************************************
changed: [clickhouse-01]

PLAY RECAP *************************************************************************************************************************************
clickhouse-01              : ok=6    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
```
$ ansible-playbook -i inventory/ site.yml --private-key=id_rsa --diff

PLAY [Install Clickhouse] **********************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ******************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] *************************************************************************************
changed: [clickhouse-01]

TASK [Create database] *************************************************************************************************
ok: [clickhouse-01]

RUNNING HANDLER [Start clickhouse service] *****************************************************************************
changed: [clickhouse-01]

PLAY [Install Vector] **************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [clickhouse-01]

TASK [Get vector distrib] **********************************************************************************************
ok: [clickhouse-01]

TASK [Install Vector packages] *****************************************************************************************
changed: [clickhouse-01]

RUNNING HANDLER [Start vector service] *********************************************************************************
changed: [clickhouse-01]

PLAY RECAP *************************************************************************************************************
clickhouse-01              : ok=9    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
```
$ ansible-playbook -i inventory/ site.yml --private-key=id_rsa --diff

PLAY [Install Clickhouse] **********************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ******************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] *************************************************************************************
ok: [clickhouse-01]

TASK [Create database] *************************************************************************************************
ok: [clickhouse-01]

PLAY [Install Vector] **************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [clickhouse-01]

TASK [Get vector distrib] **********************************************************************************************
ok: [clickhouse-01]

TASK [Install Vector packages] *****************************************************************************************
ok: [clickhouse-01]

PLAY RECAP *************************************************************************************************************
clickhouse-01              : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.  
https://github.com/sad1sm/ansible/blob/08-ansible-02-playbook/playbook/README.md  
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
