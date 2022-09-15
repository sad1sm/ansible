# Домашнее задание к занятию 8.4

## Основная часть

Наша основная цель - разбить наш playbook на отдельные roles. Задача: сделать roles для clickhouse, vector и lighthouse и написать playbook для использования этих ролей. Ожидаемый результат: существуют три ваших репозитория: два с roles и один с playbook.


1. Создать в старой версии playbook файл `requirements.yml` и заполнить его следующим содержимым.  

    Создал и заполнил:

   ```yaml
    - src: git+ssh://git@github.com/sad1sm/vector-role.git
      version: main
    - src: git+ssh://git@github.com/sad1sm/lighthouse-role.git
      version: main
    - src: git+ssh://git@github.com/sad1sm/clickhouse-role.git
      version: main
   ```

2. При помощи `ansible-galaxy` скачать себе эту роль.

    > ansible-galaxy install -r requirements.yml -p roles --force

```
Starting galaxy role install process
- extracting vector-role to /home/f1tz/netology/ansible08/playbook/roles/vector-role
- vector-role (main) was installed successfully
- extracting lighthouse-role to /home/f1tz/netology/ansible08/playbook/roles/lighthouse-role
- lighthouse-role (main) was installed successfully
- extracting clickhouse-role to /home/f1tz/netology/ansible08/playbook/roles/clickhouse-role
- clickhouse-role (main) was installed successfully
```
3. Создать новый каталог с ролью при помощи `ansible-galaxy role init vector-role`.  
>Создал.
4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`.  
>Разнес.
5. Перенести нужные шаблоны конфигов в `templates`.
>Перенес.
6. Описать в `README.md` обе роли и их параметры.  
>Всё описал.
7. Повторите шаги 3-6 для lighthouse. Помните, что одна роль должна настраивать один продукт.
> Сделано.
8. Выложите все roles в репозитории. Проставьте тэги, используя семантическую нумерацию Добавьте roles в `requirements.yml` в playbook.
> Не стал добавлять теги, так как считаю что в main/master должна быть актуальная версия.
9. Переработайте playbook на использование roles. Не забудьте про зависимости lighthouse и возможности совмещения `roles` с `tasks`.
> Сделано.
10. Выложите playbook в репозиторий.
> Выложил.
11. В ответ приведите ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.  
Роли:  
https://github.com/sad1sm/lighthouse-role  
https://github.com/sad1sm/clickhouse-role  
https://github.com/sad1sm/vector-role  
Плейбук:  
https://github.com/sad1sm/ansible/tree/08-ansible-02-playbook