# Oracle. Индексы

### Источники
https://oracle-dba.ru/docs/architecture/indexes/

### Виды индексов
| Название                                     | Суть | Плюсы                                       | Минусы               | Частота использования |
|----------------------------------------------|------|---------------------------------------------|----------------------|-----------------------|
| B-tree                                       |      | Хороши для данных с высокой кардинальностью<br/>Хороши для баз данных OLTP<br/>Легко обновляются | Занимают много места |                       |
| Битовый индекс<br/>(bitmap index)            |      | Хороши для данных с низкой кардинальностью<br/>Хороши для приложений хранилищ данных OLAP<br/>Используют, относительно мало места| Трудно обновляются| |
| С реверсированным ключом<br/>(reverse index) |      | | | |
| Со сжатым ключом                             |      | | | |