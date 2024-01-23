# Oracle. Партиционирование(Секционирование)

### Источники
https://codernet.ru/articles/drugoe/oracle_particzirovanie_tablicz_kak_upravlyat_sekcziyami/

## Методы
| Метод                                                                  | Суть                                                                          | Плюсы | Минусы | Частота использования |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------|-------|--------|-----------------------|
| По диапазону значений (range partitioning)                             | Разбиваем по диапазону значений                                               |       |        | +++                   |
| По спискам значений (list partitioning)                                | Разбиваем по заранее известному списку возможных значений конкретного столбца |       |        | +++                   |
| По хеш-значению (hash partitioning)                                    | Разбивка по хеш-значениям                                                     |       |        |                       |
| По ссылочным значениям (reference partitioning)                        |                                                                               |       |        |                       |
| По интервалу (interval partitioning)                                   |                                                                               |       |        |                       |
| Составное с хеш-подсекционированием (range-hash composite partitioning) |                                                                               |       |        |                       |
| Составное со списочным подсекционированием (range-list composite partitioning) |                                                                               |       |        |                       |



### По диапазону значений (range partitioning)
```
CREATE TABLE MY.NEWTABLE ( ISN NEWNUMBERS. UPDATED NEWDATES) TABLESPACES HSTNEWDATA
PARTITION BY RANGE (ISN)
    (PARTITION PARTISAN_01 VALUE LESS THAN (1500),
    PARTITION PARTISAN_02 VALUE LESS THAN (2500),
    PARTITION PARTISAN_03 VALUE LESS THAN (3500),
    PARTITION PARTISAN_MAXIMUM VALUE LESS THAN (MAXVALUES)
    ) ENABLE ROW MOVEMENT;
```
ENABLE ROW MOVEMENT - позволяет серверу менять rowid

### По спискам значений (list partitioning)
Пример:
```
CREATE TABLE sales_by_region ( 
     product_id     NUMBER(6), 
     quantity_sold  INTEGER, 
     sale_date      DATE, 
     store_name     VARCHAR(30), 
     state_code     VARCHAR(2) 
) 
 
     PARTITION BY LIST (state_code) 
( 
     PARTITION region_east 
        VALUES ('CT','MA','MD','ME','NH','NJ','NY','PA','VA'), 
     PARTITION region_west 
        VALUES ('AZ','CA','CO','NM','NV','OR','UT','WA'), 
     PARTITION region_south 
        VALUES ('AL','AR','GA','KY','LA','MS','TN','TX'), 
     PARTITION region_central 
        VALUES ('IA','IL','MO','MI','ND','OH','SD') 
 )
```
Чтение определенной партиции:
```
SELECT * FROM sales_by_region PARTITION(region_east)
```
Добавление партиции:
```
ALTER TABLE sales_by_region 
   ADD PARTITION region_nonmainland VALUES ('HI','PR')
```
Модификация партиции:
```
ALTER TABLE sales_by_region 
   MODIFY PARTITION region_central 
      ADD VALUES ('OK','KS')
```
Просмотр списка партиций:
```
SELECT TABLE_NAME,PARTITION_NAME, PARTITION_POSITION, HIGH_VALUE FROM USER_TAB_PARTITIONS WHERE TABLE_NAME ='SALES_BY_REGION'
```
вернёт:

| TABLE_NAME       | PARTITION_NAME | PARTITION_POSITION | HIGH_VALUE                                            |
|------------------|-------------|--------------------|-------------------------------------------------------|
| SALES_BY_REGION  | REGION_EAST | 1                  | 	'CT', 'MA', 'MD', 'ME', 'NH', 'NJ', 'NY', 'PA', 'VA' |
| SALES_BY_REGION  | REGION_WEST | 2                  |	'AZ', 'CA', 'CO', 'NM', 'NV', 'OR', 'UT', 'WA'|
| SALES_BY_REGION  | REGION_SOUTH | 3                  |	'AL', 'AR', 'GA', 'KY', 'LA', 'MS', 'TN', 'TX'|
| SALES_BY_REGION  | REGION_CENTRAL| 4                  | 	'IA', 'IL', 'MO', 'MI', 'ND', 'OH', 'SD'|


### По хеш-значению (hash partitioning)  
```
CREATE TABLE MY.NEWTABLE (TASKSISN NEWNUMBERS, OBJECTISN NEWNUMBERS, K PARAMETRS NEWNUMBERS,
CONSTRAINT NEWPK_LISTIN PRIMARY NEWKEY(TASKSISN,OBJECTISN,OBJECTROWID,K PARAMETRS)
) MYORGANIZATION INDEX INCLUDING PARAMETRS OVERFLOW PARTITION BY HASH (TASKSISN) PARTITIONS 24
```

### По хеш-значению (hash partitioning)  