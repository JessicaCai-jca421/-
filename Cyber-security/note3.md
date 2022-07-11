# Internet Security and Privacy

Some communication mediums媒介 are unsafe
What can be eavesdropped upon?什么可以被窃听？
● Air (for broadcast messages such as wireless) 广播，无线
● Copper wires (vampire tap)铜线
● Optical fiber光纤
● Devices (phones, computers, etc.)设备
Our goals:
● 机密性–保护数据包免受窃听，秘密信息会通过网络传输

● 完整性–防止传输中的数据包修改 MIMT attack

● 真实性–证明发送者的身份 its not man in the middle sending msgs, but the real sender sending msgs

加密：给数据进行加密码保护，通常都是打开是输入密码，还有隐藏、伪装等效果。市面上加密类的软件也很多，可以根据自己的具体需求进行挑选。


解密：解除密码保护，也就是恢复未加密时的状态，变成正常的数据。如果不想让你的文件继续保持加密效果，那就可以选择解密，来让它不再受保护。有的可以临时解密，使用的时候是解密状态，关闭后会自动恢复加密状态这种。

### Cryptography密码学

crypto就是密码学， 密码系统包括：

- Keys
- Encryption mechanism加密机制
- Decryption mechanism解密机制

Kerckhoffs’ Principle states that: 密码系统的密钥应该是隐藏的，但机制应该是公开的

### 加密和解密

场景：A希望将明文（plaintext）M发送给B，但不希望攻击者在M通过不安全介质（红色）时看到它。A and B both already know some key K

1. 使用加密机制Enc（）和密钥K，A将M加密为密文（ciphertext）EncK（M）。
2. A sends EncK (M) across the channel.
3. B接收EncK（M），并使用解密过程Dec（）和密钥K对其进行解密
4. Dec（Enc（M））=M；B接收明文消息M。
   1. this is end-to-end encryption(gmail不是，facebook和WhatsApp)



Simple system：该密码系统的凯撒密码问题：

● 密文重复：如果看到BUUBDL“MPOH B”，然后看到EFGFOE“MPOH B”，会怎么样？

● 密钥更新：为了安全起见，我们应该经常更新密钥。我们怎么能这样做？

● 短密钥长度：加密/解密机制有多少种可能性？

● 频率分析：如果字母“F”在密文中出现得最频繁，它是什么意思？"F" probably correspond to "E"b/c"E" 是英文字母中最常见的

##### Solving the Ciphertext Repetition Problem解决密文重复问题

使用初始化向量（IV）：block cipher分组密码

- IV“修改”加密密钥
- 每条消息必须有不同的IV
- 即使使用相同的密钥和明文，不同的IV也会产生不同的密文
- IV与消息一起公开发送–攻击者是否看到它并不重要
- since IV is public, couldn't anyone that incercept it just apply the shift and get the original cipher?既然IV是公共的，难道任何接受它的人都不能应用移位并获得原始密码吗？
- if just apply the shift of the IV, you still haven't broken the key. the point of the IV is only to prevent the deplicate cipher text problem. if you know the IV, and you apply it, you still haven't broken whatever the key have done.  In most cruptosystems, you actually can't  cancel out just the IV-如果只应用IV的移位，您仍然没有断开钥匙。IV的目的只是为了防止复杂的密文问题。如果你知道IV，并且使用它，你仍然没有破坏钥匙所做的任何事情。在大多数粗糙的系统中，你实际上不能仅仅取消静脉注射

##### Solving the Key Update Problem

找一个安全的渠道来交付钥匙

● 手工交付的文件、卡片

● 不适用于计算机系统

公钥加密

● 在PKE中，加密密钥（public）和解密密钥（private）不同

● 这可用于在不安全通道上创建安全通道

● 仅通过通道发送加密密钥

PKF传送key， AES传输msg

##### Solving the Frequency Analysis Problem

A lot of home-made crypto has this problem 

我们无法轻松做到这一点——所有替换密码对频率分析（密码）都很弱

● 一种建议的解决方案（维格纳密码）：使用键根据字母的位置移动不同的字母

● e、 g.键=狗（4 15 7），然后将第一个字母移动4，

第二乘15，第三乘7，第四乘4，第五乘15。。。

● 轻松击败！（如何？）

● 更广泛的密码分析可以击败几乎所有的“自制”密码

##### Symmetric Key Encryption (SKE)

also called secret key encryption

● 双方都知道一个密钥的一种密码系统。

The asymmetric key encryption: only one party can do the encryption, only one party can do the decryption.(Technically, any party can encrypt)警告：no private key encryption

● 如果密钥为K，则加密和解密算法为EncK（）和DecK（）。

● EncK（）和DecK（）是公共的，但K必须是秘密的。

● EncK（M）不应显示K或M。

● 双方可以加密和解密。我们将讨论三种类型：OTP、流密码和分组密码

##### Scytale 密码棒

第一个SKE用于斯巴达军队

密码棒是个可使的传递讯息字母顺序改变的工具，由一条加工过、且有夹带讯息的皮革绕在一个木棒所组成。

##### Enigma machine

用于加密和解密文件的密码机

● 关键是转子位置

● 码本包含初始位置

1） 设置为初始位置

2） 键入新职位

3） 将机器设置到新位置

4） 键入消息

##### One-Time Pad

Enigma isn't OTP b/c it didn't have a truly uniform random bit generation

**一次性**密匙( **OTP** ) 是一种无法破解的加密技术，但需要使用不小于所发送消息的一次性预共享密钥。在这种技术中，明文与随机密钥（也称为一次性密匙）配对。然后，通过使用模加法将明文的每个位或字符与来自填充的相应位或字符组合来加密明文的每个位或字符。

“完全”信息安全，如果：

● 关键点是真正一致随机的

● 钥匙只能使用一次

（为什么它非常安全？）现在也无法破解

但在实际操作上却有着以下的问题：
用以加密的文本，也就是一次性密码本，必须是无特定规律的。它可以是一串随机数字，一句话，或者一本英文名著。
它必须至少比被加密的文件等长。
用以加密的文本（密码本）只能用一次，且必须对非关系人小心保密，不再使用时，用以加密的文本应当要销毁，以防重复使用。

##### Stream cipher流密码

从随机种子生成任意长度的密钥流Generates keystream of any length from random see

流密码基本思想：
密钥流发生器f产生zi=f(k,σ \sigmaσi)，即种子密钥k产生密钥流z=z0z1z2…
加密y=y0y1y2…=Ez0(x0)Ez1(x1)Ez2(x2)…
有内部记忆原件的为流密码，否则分组密码
内部记忆原件状态σ \sigmaσi独立于明文的称同步流密码，否则自同步流密码
同步流密码加密器=滚动密钥生成器+加密变换器
二元加法流密码，加密变换yi=zi⨁ \bigoplus⨁xi
一次一密是加法流密码原型，若zi=ki,则加法流密码就退化成一次一密
密钥流序列性质：极大的周期、良好的统计特性、抗线性分析、抗统计分析
一次一密的密钥长度和明文一样长，流密码不是，需要种子密钥通过密钥生成器产生密钥流

##### Block cipher

与流密码的区别：

● 有一个固定的块大小（AES为128位）

● 明文被分割成这样大小的块

● 我们加密每个块以生成密文

● 每个块使用“相同（）”键

我们必须改变一些事情，或者我们遇到了密文重复问题（same plaintext+same key= same ciphertext

加密方式- use an IV somewhere, a mode of encryption is a specific terminology used for block cipher

我们使用该模式来避免块之间的密文重复问题：

● 电子码本（ECB）：所有密钥均相同（无密文重复防御）

● 密码块链接（CBC）：在加密之前，明文块X与密文块（X-1）异或

● 计数器（CTR）：明文块X与“keyblock”X异或，由计数器X与IV加密生成

包括DES（56位）、AES（128位）

● 1998年，DES表现得太弱

● AES是当前的标准；广泛使用

● 流密码通常更快（并且可以提前生成密钥流）
