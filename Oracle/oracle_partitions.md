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
Пример:
```
CREATE TABLE sales_range_partition ( 
    product_id       NUMBER(6), 
    customer_id      NUMBER, 
    channel_id       CHAR(1), 
    promo_id         NUMBER(6), 
    sale_date        DATE, 
    quantity_sold    INTEGER, 
    amount_sold      NUMBER(10,2) 
) 

   PARTITION BY RANGE (sale_date) 
(  
   PARTITION sales_q1_2014 VALUES LESS THAN (TO_DATE('01-APR-2014','dd-MON-yyyy')), 
   PARTITION sales_q2_2014 VALUES LESS THAN (TO_DATE('01-JUL-2014','dd-MON-yyyy')), 
   PARTITION sales_q3_2014 VALUES LESS THAN (TO_DATE('01-OCT-2014','dd-MON-yyyy')), 
   PARTITION sales_q4_2014 VALUES LESS THAN (TO_DATE('01-JAN-2015','dd-MON-yyyy')) 
)
```
Чтение определенной партиции:
```
SELECT * FROM sales_range_partition PARTITION(sales_q3_2014)
```

Добавление партиции:
```
ALTER TABLE sales_range_partition 
      ADD PARTITION sales_q1_2015 VALUES LESS THAN (TO_DATE('01-APR-2015','dd-MON-yyyy'))
```

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