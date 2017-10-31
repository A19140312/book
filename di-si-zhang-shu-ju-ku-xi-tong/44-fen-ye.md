#4.3分页
###用limit start, count分页语句。

```sql
select * from product limit start, count；
select * from product limit 866613, 20；   37.44秒
```
###对limit分页问题的性能优化方法
* 利用表的覆盖索引来加速分页查询
   * 我们都知道，利用了索引查询的语句中如果只包含了那个索引列（覆盖索引），那么这种情况会查询很快。因为利用索引查找有优化算法，且数据就在查询索引上面，不用再去找相关的数据地址了，这样节省了很多时间。另外Mysql中也有相关的索引缓存，在并发高的时候利用缓存就效果更好了。
```sql
select id from product limit 866613, 20 0.2秒
```

如果我们也要查询所有列，有两种方法：


```sql
SELECT * FROM product WHERE ID > =(select id from product limit 866613, 1) limit 20
SELECT * FROM product a JOIN (select id from product limit 866613, 20) b ON a.ID = b.id
```

