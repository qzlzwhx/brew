# mysql


# 错误

1.Every derived table must have its own alias
<br>每个派生出来的表必须都有别名，<br>

delete from buy_out_info_time_slots
where id in (select ids from (select max(id) as ids, count(id) as counts from buy_out_info_time_slots group by teacheravailable_id having counts > 1) as inner_self)
