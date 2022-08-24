# ACID Transactions, Schedules, 2PL and Lock Issues

## Transactions

A transaction is a single execution of a user program in a DBMS

- 比如在两个银行账户之间转账
- 如果资金在两个银行账户之间转账，则必须更新两个账户的记录
- 单个Transaction可能导致多个操作
- Transaction应具有ACID属性

### ACID

A： atomic，交易的全部或全部动作均未执行

- 交易通常由多个动作组成，只有在整体考虑时才是一致的
- 确保不发生部分事务

C：consistency， 数据库设计者和用户的责任

- 如果DB满足DB模式的所有约束，则它处于一致状态
- 例如，键约束、外键约束
- DBMS通常无法检测到所有不一致
- 并且可以由触发器或策略表示

例子： 输入具有重复MSP的新病历

- 允许该事务将使数据库处于不一致的状态，因此数据库拒绝该事务
- 客户只有在有账户的情况下才被允许，账户只有在有客户的情况下才被允许
- 如果DB试图维持此约束，则无法输入拥有新帐户的新客户
- 使用应用程序对约束进行建模
  - 员工不允许成为客户，但不为客户记录SIN

I：isolation，交易应独立进行，应保护其免受其他同时执行的交易的影响

- 多个交易可以交叉进行（并发处理
- 处理事务的净效果应该是，它们是按某种顺序执行的，无法保证选择了哪个序列号

D：durable，一旦事务成功完成，即使在系统崩溃的情况下，其影响也应该持续

- transcation 在内存中进行过
-
