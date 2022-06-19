# Other Indexes and Evaluation of Selections

## Skiplists

##### Optimized index structure for disk access

基本特点：

    运行相对简单

    快速的inset和delete算法

    Used by MemSQL

    在并发性方面具有优势

#### Skiplist- multiple levels of linked lists

- lowest level 按照顺序包含所有Keys
- higher level包含上一级别的一半的keys数量
  - 作为向下一级的索引
  - 运行时间O(logn)，包含range search
  - 如果元素x出现在第i层，则所有比i小的层都包含x；

Sorted Linked Lists

- inefficient 虽然是Dynamic 但是运行时间是O(n)

##### Searching

- skiplists search 一个level最多可以访问两个node
- start at top level
  - If current node == target go to bottom level and return record
  - If target < next key go down level and repeat
  - If target >=next key go right and repeat
  - If at lowest level linear search until target found or passed

##### Insertion

- 插入和删除都是problematic的，成本高昂因为要确保1/2的元素在下个level，可能需要重新排列整个列表
- Solution – relax the requirement
- Each level is expected to have ½ the nodes of the previous level
- ▪ On insertion a node is copied to the higher level with probability of 0.5
  - ▪ A randomized data structure – two skiplists with the same data inserted in the same order may differ

实际操作:

- Insert into lowest level first
- Then roll dice to see if value is inserted into higher levels
- Keep track of path to lowest levels (the visited nodes) to insert higher level nodes

##### Removal

- Removal is straightforward
  - If the entry has a tower of nodes remove the entire tower
  - Like insertion, the nodes in the search path need to be retained in the processRemoval is straightforwar

## Specialized Index Techniques

There are a number 0f specialized indexes:

- 通常与满足条件复杂的查询相关
  - 带连词和/或析取的Where子句
  - 可能涉及多个属性
- Geographic information systems
  - 部分匹配查询
  - 范围查询
  - 最近邻查询
- OLAP databases
  - 多维数据查询

##### Multiple Key Index

使用常规索引可以满足条件复杂的查询

- 通过使用复合搜索键创建索引
- 或者通过使用多个索引，检索RID并选择其交点
- 或用于析取的并集

An alternative is to create a multiple key index

- 一个属性上的索引构建在另一个属性上的索引之上
- 第一个索引是指第二个属性上的索引页
  - 对于与第一个索引不同的搜索键值，可以重复较低索引中的搜索键值

Multiple key indices work well for **range queries**

But do not support queries where data for the first attribute is missing

An alternative is a kd tree

##### kd tree

k维搜索树是用于多维数据的内存结构

- They generalize a binary search tree
- 小于节点值的值位于其左子树中
- 大于节点值的值位于其右子树中
- kd tree nodes contain an attribute name and an associated value

The levels rotate through the attributes of the tree

- With two attributes, the levels alternate between the attributes

##### Bitmap Indices

- Bitmap indices通常用于数据库中的数据挖掘和OLAP
  - Which often have low cardinality attributes
  - 变化相对较少
- Bitmap indices由多个位向量组成
  - 属性的每个可能值都有一个向量
  - A bitmap to record if a patient was a smoker would require two bit vectors
  - A bitmap on birth year might require 100 bit vectors
  - The I th bit of the index is set to 1 if the I th row of the table has the vector’s value for the attribute
- A bitmap index can speed up queries on sparse columns, that have few possible values
  - One bit is allocated for each possible value
- Use indices to answer queries

# Query Optimaization

![1655600014597](image/Part5/1655600014597.png)

![1655600600040](image/Part5/1655600600040.png)

##### Query Model and Metrics


![1655600698745](image/Part5/1655600698745.png)

Computation Model

This section covers algorithms for query operations(often more than one query)

- 单独考虑operations
- 假设数据是从磁盘读取的；实际上，情况并非总是如此，结果保留在内存中，而不是写出来

##### Unary Operators

一元运算符是具有单个操作数的运算

For SQL operators the operand is a table

- Either a base table or the result of a previous query
  - Either a base table or the result of a previous query operation

包括：

###### Selection

 A simple selection has a single condition

- 通过访问路径检索匹配的记录来满足选择
- 扫描文件并测试每条记录，以确定其是否与所选内容匹配
- 如果文件已排序且没有索引，则使用二进制搜索
- 在条件中的属性上使用索引

cost： 

- No index on the selection attribute
  - Linear search by scanning file, cost is B reads
  - 如果选择属性是候选键，则一旦找到匹配项，即可终止扫描(cost is B/2)
  - If the file is sorted use binary search to find record （▪ log2(B) + pages of matching records - 1）
- Index on the selection attribute
  - The cost is dependent on

    - The type of index – B+ tree, hash index, …
    - The height of the index
    - 与所选内容匹配的记录数
    - 索引是主索引还是次索

Cost of using an Index：

- The cost of satisfying a selection with an index is composed of
  - Number of disk reads to use the index
    - i.e. to reach the leaf / bucket that contains the data entry
    - The number of leaves / size of the bucket
  - Number of blocks of the file with records that match the selection
    - Generally larger if the index is secondary
- Assume that indices are
  - Hash index – extensible or linear
  - B+ tree index

Cost to Search Index

- B+ Tree
  - To find matching RIDs search tree
  - RIDs reside in leaf nodes
  - ***Cost:*** 1 disk read per level
- Additional leaf pages may have to be read
  - 如果索引密集或selection不平等
    - selection
- Extensible hash index
  - 读取目录
    - Probably 1 or 2 blocks
  - Read bucket(one block)
- Linear hash index
  - Read bucket
    - Bucket may have overflow blocks
  - Hash indexes only used for equality selections

Cost to Read Records

- Primary index
  - 文件按search key搜索排序
  - 在连续blocks中存储匹配的记录
  - Blocks read is number of records / records per block
    - 1 + [(records – 1) / bf(R)] (upper bound)
    - Assumes worst case

![1655604610190](image/Part5/1655604610190.png)

- Secondary index
  - 匹配的记录不会连续存储
  - 假设每个匹配记录读取一个磁盘
  - 因为记录分散在文件中
  - 对于较大的选择，可能比文件扫描更糟糕

![1655604727378](image/Part5/1655604727378.png)

![1655604775576](image/Part5/1655604775576.png)

###### Complex Selection

A complex selection is made of at least two terms connected by and (^) and or (v)

- The terms can reference different or the same attributes
- Conjunctions are more selective (and)
- Disjunctions are less selective (or)

Complex selections的满足方式与 simple selections的满足方式大致相同

- If no index on any of the selection attributes scan the file
- Use indices on selection attributes where possible
- 索引的使用取决于selection和索引的类型

###### Selections with no Disjunctions

- 如果只有一个索引可用，请使用该索引并在主存中应用其他选择
  - Either there is an index on only one of the attributes
  - 或具有引用多个选择属性的复合键的索引
  - 注意哈希索引的使用限制
- If multiple indexes are available
  - Either use the most selective
  - Or collect RIDs from leaves or buckets of indexes and take the intersection
- Selections with disjunctions are stated in conjunctive
  normal form (CNF)
  - A collection of conjuncts
  - Each conjunct consists either of a single term, or multiple terms joined by or
  - (A^ B) v C v D 三 (A v C v D) ^ (B v C v D)
  - 这允许独立考虑每个连接
- A conjunct can only be satisfied by indices if there is an index on all attributes of all of its disjunctive terms
  - If all the conjuncts contain at least one disjunction with no matching index a file scan is necessary
- Consider a selection of this form![1655605663920](image/Part5/1655605663920.png)
  - Where each of a to f is an equality selection on an attribute
- If each of the terms in either of the conjuncts has a
  matching index
  - Use the indexes to find the rids
  - Take the union of the rids and retrieve those records
  - For example, if there are indexes just on a, b, c, and e
  - Use the a, b, and c indexes and take the union of the rids
  -  Retrieve the resulting records and apply the other criteria
