# Cybersecurity

## Module 1 principles of cybersecurity

 The security related to computers or computer network

Cybersecurity 分为两个部分

- Security- Protection of assets，system,secrets
- Privacy- Protection of identity, behavior, expression
  - 表达的权力
  - 选择how people see you的权利
  - control info about you的权利

security的价格是昂贵的

### Principles of CIA

##### Confidentiality 机密性

Information is secret

The ability of a system to ensure that an asset is viewed only by authorized parities

例子：

- password被盗
- 如果一个hacker没有阅读secret info，但是他们有这么做的priviledge，这个会被认定成违反机密性吗？答案是不违反。具体来说比如他们可能得到你系统的root，and they‘re using it to make your computer a bot for the botnet, rather than your  secret info that would violate Integrity
- Eavesdropper窃听者在玩家登录时capture 账号和密码

##### Integrity 完整性

Information/System is correct

The ability of a system to ensure that an asset is modified only by the authorized parities 

例子： 

- 密码被篡改
- Man-in-the-middle: Attacker 代替用户和系统连接
- 钱被偷（online），但是如果是用户forced to shut down的servise则是违反了availability

##### Availiability 可用性

system is usable 

The ability of a system to ensure that an asset can be used by any authorized parties ，有些时候可用性和前两项是不可分割的，尤其是完整性，在很多情况下完整性被影响就会影响到可用性

例子：

- 密码被篡改
- Man-in-the-middle ，导致用户无法通过自己的密码登录系统

**Perfect CI** is achieveable by shutting down the system, not allowing anybody to access it (not available to anyone)

=> CIA principle is a tradeoff between C,I,A

###### DDOS 分布式拒绝服务攻击

多个不同machines被用一个攻击者控制，攻击相同的services- 这种情况违反了avaliablity

攻击者使用了一个通过猜测微不足道的远程访问密码创建的僵尸网络 the attacker used a botnet that was created by guessing trivial remote access passwords- 这种情况违反了integrity， 物联网设备加入僵尸网络，这些设备的所有者不应该这样做（不违反机密性：usename&password不包含任何机密信息b/c这些物联网没有要求用户更改默认用户名和密码）

![1657295252357](image/Note1-3/1657295252357.png)![1657295252357](https://file+.vscode-resource.vscode-cdn.net/d%3A/%E5%B0%8F%E8%94%A1%E9%B8%9F%E7%9A%84github/XCN-studyList/Cyber-security/image/Note1-3/1657295252357.png)

Assets: the items you value. There are  many types of assets involving hardware,sofeware,data,people processes pr conbinations of these.

Assets' values are personal ,time dependent, and often imprecise. 不精确的

![1657295434822](https://file+.vscode-resource.vscode-cdn.net/d%3A/%E5%B0%8F%E8%94%A1%E9%B8%9F%E7%9A%84github/XCN-studyList/Cyber-security/image/Note1-3/1657295434822.png)

###### Vulnerabilities 漏洞

如果你不知道用户的需求，你就无法证供的从创建一个成功的安全系统

分为两种情况

- Design
  - Wrong threat model: misunderstand, where the attacker come from(少见，因为公司的安全团队会防范)
    - 可以通过社会工程学攻击
  - Wrong user model: misunderstand what users are doing/who is the user
    - eg: giving administrative permission 比如在操作时软件申请root权限，很少有人会拒绝
  - 以上这两点did not design for security
- Implementation
  - 代码，硬件，网络等出现错误

Spiderman Rule： 能力越大责任越大

###### Privacy

- 隐私性不同于confidentiality ,我的理解是privacy是个人信息相关，而confidentiality是non-private的信息
  - 违反隐私的情况是： Welcome back ,customer. last week yoy purchased......(shopping list)
  - 违反confidentiality的情况是：Welcome back ,customer. As a reward,please use this gift code for your next purchase， 在这种情况下gift code 不是private or personal info
- The connectuon between Information and Identity
  - issues(human issues): expression, socla vulnerability,  behavorial analysis, discrimination判别
  - Protections： Anonymization匿名，disassociation分离（解除动作与人的关联disassociating the action with the perso），security

![1657296771383](https://file+.vscode-resource.vscode-cdn.net/d%3A/%E5%B0%8F%E8%94%A1%E9%B8%9F%E7%9A%84github/XCN-studyList/Cyber-security/image/Note1-3/1657296771383.png)

##### Defensive Strategy

Risk Management: A risk is something that could damage, destory,or disclose公布 data

Essential for convincing upper management to adopt security countermeasures对说服高层管理层采取安全对策至关重要

###### Quilitative Risk Analysis

- 每个公司的衡量标准不一样， what sort of indestry that you're looking at

###### Quantitative Risk Analysis

For any given risk ，都需要计算SLE单一损失预期

- SLE： Single loss expectancy= Asset Value* Exposure Factor曝光系数(% of assents exposed to the risk)

然后计算公司的ALE年化预期损失

- ALE： annual loss expectancy= SLE* annualized rate of occurrence

If you proposed countermeasure can reduce ale more than the cost of the countermeasure如果您提出的对策可以减少ale，而不是对策的成本就很好

### Principles of Secure Design

###### Security by design 

Security （还有漏洞）should be sonsidered starting from the design phase 结算

- 如果在设计阶段没有设计安全性，那么在之后添加将会是非常困难的，因为在此之前攻击就有可能产生了
  - might not integrited with the design
  - the modeling might be poor
  - cannot add security later b/c the producted already shipped
  - sms 是未加密的（not encryption）

### Saltzer and Schroeder's Principles of secure design

##### * Open Design

  the system's design should be openly to everyone

The design should not be secret. The mechanisms should not depend on the ignorance of potential attackers, but rather on the possession of specific, more easily protected, keys or passwords. This decoupling of protection mechanisms from protection keys permits the mechanisms to be examined by many reviewers without concern that the review may itself compromise the safeguards. 

- The secrets should not be in design (algorithm, how we allow people to login, how we authenticate people who are logging in), but can be the password, encryption keys
- 与隐蔽性的安全相反opposite of security through obscurity默默无闻
  - hide detials of the implementation to prevent comprromising analysis隐藏实现细节以防止入侵分析

    - reserve engineering- taking the product,then figutr out how it was made(ie source code
    - 通常不会生效因为reserve engineering的存在
    - 有时生效的原因是exposing details make the product more attractive to hackers
  - Examples of security through obsurity隐蔽的安全

    - Terms of service+ lawsuits barring reverse engineering 禁止诉讼
    - cryptosystems where the algorithms are secret 算法保密的密码系统
    - gag order禁言令- people were forbidden to talk about the lawsuits and their research
- Linus Torvalds law- Given enrough eyeballs, all bugs are shallow
  - 但是也会出现开源代码没有人检查的情况比如： Heartbleed - serious open- source software bug
    - can steal the memory of any machine on the internet

##### * Economy of Mechanism

在这里的economy指的是 efficiency/simplicity

The system should be simple enough to undersatnd and analysis

但它值得强调保护机制，原因是：在正常使用过程中不会注意到导致不需要的访问路径的设计和实现错误

- KISS 原则： Keep it simple/stupid
- 对安全分析有帮助
- 鼓励好的设计
- Complicated solutions are bypassed by simple workarounds复杂的解决方案被简单的变通方法所绕过 ex: Juicero

##### * Least common Mechanism 最少相同机制

Minimize the amount of mechanism common to more than one user and depended on by all usersThe amount of share 应尽量减少所有用户所依赖的共享机制数量

最少通用机制 (LCM) 的目标是管理错误和成本。大多数有用的计算机系统都带有大型可共享代码库，以帮助程序员和用户使用常用的功能。（这些库所包含的内容这些年来急剧增长。）这些库是代码的集合，以及必须由某人编写和调试的代码。编写安全代码既困难又昂贵。编写可以有效保护自己的代码是一项挑战，如果系统充满了以特权运行的大型库，那么其中一些库将存在暴露这些特权的错误。

- 最小化伤害， damage can be covered much more easily
- Examples for web hosting
  - use mutiple data centers
  - backup your database
  - localize computations 计算机本地化
    - Do not use all computations on the same deviices
    - 比如C语言的标准数据库中就存在很多不好的函数：no bounds checking

##### * Least Privilege 最小特权

每个程序和系统的每个用户都应该使用完成工作所需的最少权限集进行操作 Every program and every user of the system should operate using the least set of privileges necessary to complete the job.

该原则限制了事故或错误可能导致的损害。它还将特权程序之间的潜在交互次数减少到正确操作的最低限度，从而不太可能发生无意的、不需要的或不正确的特权使用。因此，如果出现与滥用特权有关的问题，则必须审计的程序数量将被最小化

##### * Separation of Privileges

  The system should grant permission based on mulitiple conditions

在可行的情况下，需要两把钥匙才能解锁的保护机制比只允许访问者只需一把钥匙的保护机制更加稳健和灵活。

- 比如2-step verification

##### * Complete Mediation 完整的调节

All accesses should be checked

必须检查对每个对象的每次访问权限。当系统地应用这一原则时，它是保护系统的主要基础。它强制访问控制的系统范围视图，除了正常操作外，还包括初始化、恢复、关闭和维护。

- Login to system -> view grades -> find file 这每一步都需要mediation
- 跟TOCTTOC， cookie Manipulation相关

##### * Fail- safe defaults

Upon failure, the system should revert to a secure default 一旦失败，系统应恢复到 到一个安全的默认值

- POODLE (Padding Oracle on Downgraded Legacy Encryption): 在降级的遗留加密上填充oracle
  ●SSL被更新为TLS，以消除一个填充Oracle漏洞
  ● 客户端可以迫使服务器再次降级为SSL
  ● 难以修复，因为一些客户确实没有 更新到TLS

##### * Psychological Acceptability

Security should be intuitive to the human psyche.

安全方面的大多数问题并不涉及如此重要的选择，但都必须与心理可接受性测试进行权衡。

![1657318255720](image/Note1-3/1657318255720.png)

##### * Work factor

Consider how much effort an attacker needs to expend to attack the system.考虑攻击者需要花费多少精力来攻击该系统。

##### *  Compromise recording妥协记录

Always record compromise events.有时会有人提出，可以用可靠地记录信息泄露事件的机制来代替完全防止损失的更复杂的机制。

###### 几个例题： 

- 为了将他的个人资料放在系统上，以便绕过生物特征测试，汤姆·克鲁斯潜入了一个水控制系统，撕掉了旧的个人资料驱动器，并插入了一个新的个人资料驱动器
  - 违反了fail-safe defaults 因为系统应该crash和等待一个人来查看有什么问题
- 一旦进入，他的搭档在首相的各个银行账户中窃取了约24亿英镑的信息
  - 违反了least-privilege 和complete mediation
- 我很害怕，所以我安装了推荐的防病毒软件。一扇窗户弹出请求管理员权限，我同意。
  - 违反了psycho acceptability和economy of mechanism（因为The antivirus is so complicated that I cant really analysis it
- 反病毒实际上是一种病毒，它利用glibc中的缓冲区溢出
  - 违反了least common mechanism

###### 一些小单词：

Compromised 遭受入侵

Eavasdropper 窃听

sms= short message service 短讯服务

cyberbully 网络欺凌, cyberstalking 网络跟踪

# Software Security

The best way to create a malware is not create malware, is to download other people's malware恶意软件

![1657319690916](image/Note1-3/1657319690916.png)

### Unintentional Flaws

分为两种

- Local application flaws
  - Buffer overread, buffer overflow, TOCTTOU
  - 缺陷存在于program‘s code，通常exploited within the same system, 通常从网络上被开发(exploited)， a pocket sends there the software reads the packet and the get hacked
- Web application flaws
  - XSS, XSRF, SQL Injection
  - code accessible from the internt
  - only exploited over the net b/c they're based on network technology,web brower technlogy
- eg:一个软件既有本地应用程序也有网络应用程序，从互联网上可以访问的代码中有一些弱点，这就导致了问题的出现

##### Buffer overread
