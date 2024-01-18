# Greenplum Cheatsheet ADMIN

### Использование дискового пространства
```
select * from gp_toolkit.gp_disk_free
```
| Название поля | Описание                 |
|---------------|--------------------------|
| dfsegment     | номер сегмента           |
| dfhostname    | id хоста                 |
| dfdevice      | id диска                 |
| dfspace       | свободное место в байтах |

### Роли
**Список ролей**
```
select * from pg_roles;
```
| Название поля | Описание |
|----|----|
| rolsuper | права root |
| rolinherit | |
| rolinherit | |

**Добавление роли**
```
CREATE USER pelevin_vo WITH PASSWORD 'route666';
```
