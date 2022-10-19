# increase speed optimizer_switch

## check current

````
mysql> SELECT @@optimizer_switch\G
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on
1 row in set (0.00 sec)
mysql>
````


ให้ทำการสังเกตุข้อมูลในส่วนของ derived_merge=on
และทำการ Disable Option optimizer_switch : derived_merge=off
Disable Option : optimizer_switch 



## Edit Config database

nano /etc/my.cnf

````
[mysqld]
optimizer_switch=derived_merge=off
````


## check 
````
SELECT @@optimizer_switch\G
````

ให้ทำการสังเกตุข้อมูลในส่วนของ derived_merge จะพบว่าค่า Configuration ถูกเปลี่ยนแปลงเป็น derived_merge=off


