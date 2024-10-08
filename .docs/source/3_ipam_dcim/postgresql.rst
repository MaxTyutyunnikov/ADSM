.. meta::
   :http-equiv=Content-Type: text/html; charset=utf-8

Немного о PostgreSQL
====================

Для подключения к серверу: 

    .. code-block:: bash

       psql -U *username* -h *hostname* *db_name*

Например: 

    .. code-block:: bash

       psql -U netbox -h localhost netbox

Для вывода всех таблиц: 

    .. code-block:: bash

       /dt

Для выхода: 

    .. code-block:: bash

       /q

Для дампа БД: 

    .. code-block:: bash

       pg_dump -U *username* -h *hostname* *db_name* > netbox.sql

Если не хочется каждый раз вводить пароль:

    .. code-block:: bash

       echo *:*:*:*username*:*password* > ~/.pgpass
       chmod 600 ~/.pgpass

Если у вас есть своя инсталляция и не хочется вносить всё руками, можно просто сделать так, взяв дамп текущей БД NetBox `тут <https://github.com/eucariot/ADSM/blob/master/docs/source/3_ipam/netbox_initial_db.sql>`_:

    .. code-block:: bash

       psql -U *username* -h *hostname* *db_name* < netbox_initial_db.sql

Если предварительно нужно дропнуть все таблицы (а сделать это придётся), то можно подготовить заранее файл:

    .. code-block:: bash

       psql -U *username* -h *hostname* *db_name*
       \o drop_all_tables.sql
       select 'drop table ' || tablename || ' cascade;' from pg_tables;
       \q
       psql -U *username* -h *hostname* *db_name* -f drop_all_tables.sql
