---
title: 阿里巴巴Java编程规范
date: 2018-02-13 09:54:12
categories: server
tags: [java] 
---
### 前言：
<center>关于《阿里巴巴Java开发手册》</center>



你是否曾因Java代码规范版本纷杂而无所适从？

你是否想过代码规范能将系统故障率降低20%？

你是否曾因团队代码风格迥异而协同困难？

你是否正在review一些原本可以避免的故障？

你是否无法确定自己的代码足够健壮？ 

<!--more-->

#### 码出高效，码出质量！
相比C++代码规范业界已经达成共识，Java代码规范业界比较混乱，我们期待这次发布的Java代码规范能够给业界带来一个标准，促使整体行业代码规范水平得到提高，最终能够帮助企业和开发者提升代码质量和降低代码故障率。

#### 阿里出品，质量保证！

阿里Java技术团队一手打造出Dubbo、JStorm、Fastjson等诸多流行开源框架，部分已成为Apache基金会孵化项目；

阿里在Java后端领域支撑起全球访问量最大的服务器集群；

Java代码构建的阿里双11业务系统订单处理能力达到17.5万笔/秒；

到目前已累计数亿行高并发、高稳定性的最佳Java代码实践；

……

此次公开的Java开发手册正是出自这样的团队，近万名阿里Java技术精英的经验总结，并经历了多次大规模一线实战检验及完善，铸就了这本高含金量的阿里Java开发手册。该手册以Java开发者为中心视角，划分为编程规约、异常日志规约、MYSQL规约、工程规约、安全规约五大块，再根据内容特征，细分成若干二级子目录。根据约束力强弱和故障敏感性，规约依次分为强制、推荐、参考三大类。此套规范不仅能让代码一目了然， 更有助于加强团队分工与合作、真正提升效率。 


#### 无规矩不成方圆 无规范不能协作

众所周知，制订交通法规表面上是要限制行车权，实际上是保障公众的人身安全。试想如果没有限速，没有红绿灯，没有规定靠右行驶，谁还敢上路行驶。 

同理，对软件来说，适当的规范和标准绝不是消灭代码内容的创造性、优雅性，而是限制过度个性化，以一种普遍认可的方式一起做事，降低故障率，提升协作效率。开发手册详细列举如何开发更加高效，更加容错，更加有协作性，力求知其然，更知其不然，结合正反例，提高代码质量。比如，异常日志处理时的各种不规范行为；集合转换的各种坑；创建线程池出现的等待队列OOM等。 

#### 阿里技术资深大咖联袂推荐

阿里高级研究员多隆：工程师对于代码，一定要“精益求精”，不论从性能，还是简洁优雅，都要具备“精益求精”的工匠精神，认真打磨自己的作品。 

阿里研究员毕玄：一个优秀的工程师和一个普通工程师的区别，不是现在满天飞的架构图，他的功底就是体现在他写的每一行代码上。 

阿里研究员玄难：代码是软件工程里面的产品设计、系统架构设计等工作的最后承载体，代码的质量决定了一切工作的成败。 

阿里巴巴B2B事业群CTO李纯：好的软件产品离不开工程师高质量的代码及相互间顺畅的沟通与合作。简单，适用的代码规约背后所传递的是技术上的追求卓越、协同合作的精神，是每个技术团队不可缺失的重要利器。 

阿里研究员、HipHop作者：赵海平（花名：福贝）：程序员是创造个性化作品的艺术家，但同时也是需要团队合作的工种。个性化应尽量表现在代码效率和算法方面，牺牲小我，成就大我。

#### 拥抱规范，远离伤害！
     
开发的同学们赶紧行动起来，遵守代码规范，你好，我好，大家好！


##### 传送门
1. 原文: https://yq.aliyun.com/articles/69327?spm=5176.100239.blogcont69327.158.xUUgiz&p=2#comments
2. 「阿里巴巴编码规范」考试认证 : https://edu.aliyun.com/certification/cldt02 
3. 点击下载《阿里巴巴Java开发手册》(纪念版): https://yq.aliyun.com/attachment/download/?id=4942
4. IDE插件下载 : https://github.com/alibaba/p3c


### 以下记录以下自己需要注意的一些规范, `遵守代码规范，你好，我好，大家好！`

#### 编程规约

#### 命名风格
1. 【强制】

     抽象类命名使用 Abstract 或 Base 开头； 

     异常类命名使用 Exception 结尾； 
    
     测试类命名以它要测试的类的名称开始，以 Test 结尾。
2. 【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
   
   正例： `MAX_STOCK_COUNT`
   
   反例： `MAX_COUNT`
3. 【强制】 POJO 类中布尔类型的变量，都不要加 is，否则部分框架解析会引起序列化错误。
   
   反例： 定义为基本数据类型 Boolean isDeleted； 的属性，它的方法也是 isDeleted()， RPC 框架在反向解析的时候， “以为”对应的属性名称是 deleted，导致属性获取不到，进而抛出异
                                                                常。
4. 【强制】杜绝完全不规范的缩写， 避免望文不知义。

   反例： AbstractClass“缩写” 命名成 AbsClass； condition“缩写” 命名成 condi，此类随
   意缩写严重降低了代码的可阅读性。
   
5. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词
   组合来表达其意。
   
   正例： 从远程仓库拉取代码的类命名为 PullCodeFromRemoteRepository。
   
   反例： 变量 int a; 的随意命名方式。
   
6. 【推荐】接口类中的方法和属性不要加任何修饰符号（public 也不要加） ，保持代码的简洁
   性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是
   与接口方法相关，并且是整个应用的基础常量。
   
   正例： 接口方法签名： void f();
   
   接口基础常量表示： String COMPANY = "alibaba";
   
   反例： 接口方法定义： public abstract void f();
   
   说明： JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默
   认实现。
   
7. 【参考】枚举类名建议带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。
   说明： 枚举其实就是特殊的常量类，且构造方法被默认强制是私有。
   
   正例： 枚举名字为 ProcessStatusEnum 的成员名称： SUCCESS / UNKOWN_REASON。
   
8. 【参考】各层命名规约：

   > A) Service/DAO 层方法命名规约
   
   1） 获取单个对象的方法用 get 做前缀。
   
   2） 获取多个对象的方法用 list 做前缀。
   
   3） 获取统计值的方法用 count 做前缀。
   
   4） 插入的方法用 save/insert 做前缀。
   
   5） 删除的方法用 remove/delete 做前缀。
   
   6） 修改的方法用 update 做前缀。
   
   > B) 领域模型命名规约
   
   1） 数据对象： xxxDO， xxx 即为数据表名。
   
   2） 数据传输对象： xxxDTO， xxx 为业务领域相关的名称。
   
   3） 展示对象： xxxVO， xxx 一般为网页名称。
   
   4） POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。
   
#### 常量定义
1. 【强制】不允许任何魔法值（即未经定义的常量） 直接出现在代码中。

   反例： String key = "Id#taobao_" + tradeId;
   
   cache.put(key, value);
2. 【推荐】不要使用一个常量类维护所有常量， 按常量功能进行归类，分开维护。

   说明： 大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。
   
   正例： 缓存相关常量放在类 CacheConsts 下； 系统配置相关常量放在类 ConfigConsts 下。
#### 代码格式
1.示例
```java
public static void main(String[] args) {
    // 缩进 4 个空格
    String say = "hello";
    // 运算符的左右必须有一个空格
    int flag = 0;阿里巴巴 Java 开发手册
    // 关键词 if 与括号之间必须有一个空格，括号内的 f 与左括号， 0 与右括号不需要空格
    if (flag == 0) {
        System.out.println(say);
    }
    // 左大括号前加空格且不换行；左大括号后换行
    if (flag == 1) {
        System.out.println("world");
        // 右大括号前换行，右大括号后有 else，不用换行
    } else {
        System.out.println("ok");
        // 在右大括号后直接结束，则必须换行
    }
}
```
2.【强制】 注释的双斜线与注释内容之间有且仅有一个空格。

   正例： // 注释内容， 注意在//和注释内容之间有一个空格。
   
3.【强制】单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则：

   1） 第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进，参考示例。
   
   2） 运算符与下文一起换行。
   
   3） 方法调用的点符号与下文一起换行。
   
   4） 方法调用时，多个参数， 需要换行时， 在逗号后进行。
   
   5） 在括号前不要换行，见反例。
   
   正例：
```java
   StringBuffer sb = new StringBuffer();
   // 超过 120 个字符的情况下，换行缩进 4 个空格， 点号和方法名称一起换行
   sb.append("zi").append("xin")...
       .append("huang")...
       .append("huang")...
       .append("huang");
```
   反例：
   ```java
   StringBuffer sb = new StringBuffer();
   // 超过 120 个字符的情况下，不要在括号前换行
   sb.append("zi").append("xin")...append
       ("huang");
   // 参数很多的方法调用可能超过 120 个字符， 不要在逗号前换行
   method(args1, args2, args3, ...
      , argsX);
   ```
4.【强制】方法参数在定义和传入时，多个参数逗号后边必须加空格。

   正例： 下例中实参的"a",后边必须要有一个空格。
   
   method("a", "b", "c");
5.【推荐】方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义
   之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。
   说明： 没有必要插入多个空行进行隔开
#### 注释规范
1. 【强制】类、类属性、类方法的注释必须使用 Javadoc 规范，使用/**内容*/格式，不得使用
   // xxx 方式。
   
   说明： 在 IDE 编辑窗口中， Javadoc 方式会提示相关注释，生成 Javadoc 可以正确输出相应注
   释； 在 IDE 中，工程调用方法时，不进入方法即可悬浮提示方法、参数、返回值的意义，提高
   阅读效率
2. 【强制】所有的抽象方法（包括接口中的方法） 必须要用 Javadoc 注释、除了返回值、参数、
   异常说明外，还必须指出该方法做什么事情，实现什么功能。
   
   说明： 对子类的实现要求，或者调用注意事项，请一并说明
3. 【强制】所有的类都必须添加创建者和创建日期。

4. 【推荐】与其“半吊子”英文来注释，不如用中文注释把问题说清楚。专有名词与关键字保持
   英文原文即可
5. 【推荐】代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑
   等的修改
   
   【推荐】代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑
   等的修改
   
6. 【推荐】代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑
   等的修改
   
7.  【参考】特殊注释标记，请注明标记人与标记时间。注意及时处理这些标记，通过标记扫描，
   经常清理此类标记。线上故障有时候就是来源于这些标记处的代码。
   1） 待办事宜（TODO） :（标记人，标记时间， [预计处理时间]）
   表示需要实现，但目前还未实现的功能。这实际上是一个 Javadoc 的标签，目前的 Javadoc
   还没有实现，但已经被广泛使用。只能应用于类，接口和方法（因为它是一个 Javadoc 标签） 。
   2） 错误，不能工作（FIXME） :（标记人，标记时间， [预计处理时间]）
   在注释中用 FIXME 标记某代码是错误的，而且不能工作，需要及时纠正的情况。
#### 异常处理
1. 【强制】有 try 块放到了事务代码中， catch 异常后，如果需要回滚事务，一定要注意手动回
   滚事务。
2. 【强制】有 try 块放到了事务代码中， catch 异常后，如果需要回滚事务，一定要注意手动回
   滚事务。
   
#### MySQL 数据库 建表规约
1.【强制】表达是与否概念的字段，必须使用 is_xxx 的方式命名，数据类型是 unsigned tinyint
（1 表示是， 0 表示否） 。

说明： 任何字段如果为非负数，必须是 unsigned。

正例： 表达逻辑删除的字段名 is_deleted， 1 表示删除， 0 表示未删除

2.【强制】 varchar 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长
    度大于此值，定义字段类型为 text，独立出来一张表，用主键来对应，避免影响其它字段索
    引效率。
3.【强制】表必备三字段： id, gmt_create, gmt_modified。

说明： 其中 id 必为主键，类型为 unsigned bigint、单表时自增、步长为 1。 gmt_create,
gmt_modified 的类型均为 date_time 类型，前者现在时表示主动创建，后者过去分词表示被
   动更新
4.【推荐】表的命名最好是加上“业务名称_表的作用”。
  正例： alipay_task / force_project / trade_config
5.【强制】业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引。
   说明： 不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明
   显的； 另外，即使在应用层做了非常完善的校验控制，只要没有唯一索引，根据墨菲定律，必
   然有脏数据产生
6.【强制】超过三个表禁止 join。需要 join 的字段，数据类型必须绝对一致； 多表关联查询时，
  保证被关联的字段需要有索引。
  说明： 即使双表 join 也要注意表索引、 SQL 性能
#### MySQL 数据库 SQL语句
1. 【强制】不要使用 count(列名)或 count(常量)来替代 count(*)， count(*)是 SQL92 定义的
标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。
说明： count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行