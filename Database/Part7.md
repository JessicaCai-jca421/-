# Query Optimization - Size Estimation and Join Order

## Estimating Relation Size

##### 逻辑到物理计划

对于从逻辑计划导出的每个物理计划，我们记录

- An order and grouping for operations such as joins, unions and intersections
- 逻辑规划中每个运算符的算法
- 物理计划所需的其他操作员
- 参数从一个运算符传递到下一个运算符的方式

##### 估计规则

估计结果大小所需的信息存储在system catalog中

估计大小的规则有三个： 精确的，  易于计算 和 逻辑一致性

目的是将最低成本分配给最佳计划，估计是否准确并不重要

##### Projections

Projections 后关系的大小可以根据关系的信息进行估计，其中包括属性的数量和类型

- **Sum of (column sizes) * estimated number of records**

##### Selections on Equality

选择会减小结果的大小，但不会减小每个记录的大小

如果属性是一个可预估的常数则大小为：

- T(S) = T(R) / V(R,A)
  - 其中S是选择的结果，T（R）是记录数，V（R，A）是属性A的值计数

##### Zipfian Distribution

假设一个属性的值出现的频率相同可能是不正确的，许多属性的值遵循zipfian分布

- The frequency of the ith most common item is proportional to 1/根号下i
  - 例如，如果最常见值出现1000次，则第二常见值出现1000次/根号下2=707次

##### Selections on Inequality

一个简单的规则是估计平均有一半的记录满足一个选择，或者，估计不等式返回三分之一的记录

- Alternatively assume：T(R) * (V(R,A) – 1 / V(R,A))

##### And and OR

对于AND子句，将选择视为选择的级联（cascade)

- 为每个选择应用selectivity factor或者 OR clauses
- 假设没有记录同时满足这两个条件

  - 结果的大小是每个单独条件的结果之和，或者假设选择是独立的
  - result = n*(1 – (1 – m1/n)*(1 – m2/n)),其中R有n个元组，m1和m2是满足每个条件的分数


##### 估计natural join 大小

假设一个自然连接在一个公共属性的等式上，称之为x，

- 这两个关系可以有不相交的x集， 连接为空，T（R⋈ S） =0
- x可能是S的键和R中的外键，T(R ⋈ S) = T(R）
- 在R和S的大多数记录中，x可能是相同的， T(R ⋈ S) ≈ T(R) * T(S）

##### Assumption

- Containment of value sets
  - 如果x出现在多个关系中，则其值位于固定列表x 1、x 2、x 3。。
  - 系从列表的前面获取值，并在前缀中包含所有值
  - 如果R和S包含x和V（R，x）≤ V（S，x）那么R的x的每个值也将在S中
