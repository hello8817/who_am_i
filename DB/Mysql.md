## MySQL

### 库表操作类问题

- **explain**: 学会使用 explain 关键词,查询 sql 语句的执行情况,协助进行 sql 语句优化
    - [EXPLAIN用法和结果分析](https://blog.csdn.net/why15732625998/article/details/80388236)


- **where in** 条件查询为什么效率会低?  
  在 in 后面的数据比较少的时候,使用索引,查询效率会高一些,但是 in 后面的数据量比较大的时候,in 的查询就会转为全表查询,导致查询的性能极具下降.
