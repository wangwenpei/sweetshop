SweatShop
=========


根据 git commit 提交时间，计算自己的加班时长。
计算结果为根据
[中华人民共和国劳动法](http://www.npc.gov.cn/wxzl/gongbao/2000-12/05/content_5004622.htm)
第四十四条对应策略换算后的时长


统计精确度有效时间为2015年11月之后


## 使用限制

* 仅支持 中华人民共和国 假日作息
* 时间范围为2015年11月起，早于的时间只会按普通双休处理，有需求的用户请自行 PR 完善假日
* 在项目目录中执行

## 关于精度

* 默认时间朝十晚七，简化为计算上午下午，各4小时
* 统计结果约等于实际加班时长，目前暂时忽略了以下问题：
    - 对于法定假日如国庆、春节的补偿算法，统一按200%而不是300%计算
    - 目前0-4点的 commit 会归为第二天，取决于开发者习惯，多数情况为少于实际时长
        - 前一天晚上加班时间没有任何 commit 入库
        - 开发者节假日加班至凌晨（应该比较少见）


## 准备工作


首先生成 git log。可参考 `demo/mp.prepare.sh`

```
cd <需要统计的代码仓库路径>
git log --format='%ae;%aD' > <输出路径>
cd -

```


## 开始执行

```
export SWEATSHOP_EMAIL='<commit提交时的email-01>,<commit提交时的email-02>' # 默认为作者邮箱
export SWEATSHOP_WEEKDAY='6,7' # 默认为周日休息一天
export SWEATSHOP_WORKING_TIME='9-18'  #  默认为10-19
export SWEATSHOP_DEBUG='yes'  # 查看明细工时
export SWEATSHOP_HOURS='yes'  # 只计算小时，用于评估单周是否超44小时


cat <输出路径> | python ./mapper.py | sort | python ./reducer.py
```


具体可参考 demo 中的范例




## 冷知识

* 公司强制解约，赔偿2n的赔偿金，但是，赔偿基数不能超过当地人均工资的三倍。
    2017年北京大约为2w2，上海为1w9。
    所以基本上对于一个程序员来说，萌新的超神，3年多的小牛到这个薪资很容易。
    划算点的（其实一点都不划算，特别是合同欺诈，只是从止损的角度）就是协商离职然后直接去下家吧。
    反正这个行业，上午离职，下午面试，只要你有能力，分分钟的事。

* 劳动仲裁虽然保护劳动者，但是不负责处理合同欺诈，就个人经历而言，劳动仲裁更多的是为了保护处于社会底层的劳动者。
   不管你有没有意识到，程序员真的不在社会底层，其实更多的是在中产层。
   萌新程序员其实还真没到需要有劳动纠纷的段位，在公司也没有什么核心的机密可言。
   找个好老大，如果公司坑，老大也会帮你想办法的。

* 如果存在著作权争议，可以立刻发起诉讼，法院在48小时内要进行裁决，当前后期还有的扯。


## 人生经验


### 关于劳动仲裁

其实劳动仲裁更多的不像大家想象的那样，劳动仲裁只能帮助员工找回赔偿，但是不处理欺诈。
欺诈只能走法律程序，可以先去找劳动调解室。


### 关于期权

个人建议对期权的态度，一定要当福利，不要把他看成是薪酬体系的一部分，
虽然作者身边太多朋友期权都有兑现的经历，最差最差的都能顶每年发了个年终奖吧。
牛逼的自然是如段子中一样，财务自由，成为人生赢家，走上人生巅峰。

但是，这种观点首先是错的，法律上不认可期权是严格意义的工资，只能认为是一种福利。
虽然他确实带来了变现能力，但不是薪酬部分，当然它是权利，也有价值，但不是薪酬。

此外，这种想法也会被人利用，在谈判上会用期权作为筹码，刻意压低薪资。


   
### 背书人和合同   

以前我还是比较认背书人的，主要是在作者曾经的朋友圈中，大家都信守诚实信用守则（最近看法律新学的词）
其实就以往经历而言，这些靠谱的朋友，都会把任何利益和风险，明确规范在合同中。
就个体感受而言，我认为：
  
* 如果双方都发自内心为对方着想，写在合同上其实是共赢的，毕竟合同就是要有权利有义务；
    
* 如果真如口头承诺所说能办到，那写在合同说也没什么大不了的。
   
  
有个观点是："人无所谓忠诚,只是因为背叛的筹码太低。"
很多时候，我们每个人都无法想象在巨大的经济利益面前，人会做什么事。

在经济学领域，诺贝尔经济学奖得主弗里德曼说过：“花自己的钱办自己的事，最为经济；
花自己的钱给别人办事，最有效率；花别人的钱为自己办事，最为浪费；
花别人的钱为别人办事，最不负责任。”

其实很多事情冷静下来分析一下，道理都非常浅显易懂。更多时是当局者迷。
在合同签署时，千万不要受对方的口碑、私交等因素影响作为加分项，
建议大家理智，不妨先从经济学角度冷静分析。

最后郑重建议大家，不论是达成雇佣关系或者是与人谈生意时，
一定要落实到合同中，如果不规范的事情已经发生，
那么做好取证工作，通过法律手段来维权。
但使用法律的问题最大的就是耗时比较长，一般半年起，即使能快速立案，还有案件的审核。
而通常在合同中处于弱势地位的一方，付出会更多。


其实说到底，最好的办法还是在合同条款中，明确说明，规避风险。


### 不要对离职协议等签字

如果对方确实违法了，而你又不打算放过他们，请记住，不要签任何字。
法律会保护你，但是前提是作为行为人，不要对目前的补偿方案签任何字。
后期如果有纠纷，你就只能去走法院了。没有任何机关单位能保护你的利益。


### 对创业市场悲观

前几天跟业内的朋友聊天，我们有一个共识，就是一家互联网公司，在不同阶段，不同的角色有不同的地位。

前期，技术是主角；中期，产品是主角；后期，运营是主角。

以初创公司为例，技术在互联网公司的价值会随着公司的成长，渐渐被边缘化，并不是说不重要，而是渐渐走向稳定。
这是很正常的现象，这也是自然规律。
但是，也因为如此，对于公司来说，追求利益最大化是非常合理的事情，
但这其中就很有可能会有些人愿意冒法律风险，在合同上动手脚，清理掉前期的投入人员，减少沉没成本。
从我们国家现在的法律制度看，其实违法成本很低。
而对于技术人员这种前期很重要的角色，自然就会被盯上，如果合同中有隐患、对方有合同欺诈等行为，被过河拆桥的风险是极高的。

最近的案例其实也是蛮多的，比如大辉的丁香园当时刷爆了朋友圈。
而这种案件越来越多的话，并不是只是单单给一个人或者一个公司带来影响。
期权激励这种制度在行业内势必会大大折扣。

想象一下这样的创业环境，
创业公司想创业，手上没有钱，
想通过期权制度，把员工的利益与公司的利益捆绑，大家共进退。
但是整个大环境的期权激励信用下降，没太多人愿意认可期权，而只愿意认工资。
能出的起钱的公司进场，出不起的淘汰。

剩下没钱的公司，只能去市场中捡白菜，人品好可能捡到青菜。
带领公司少走弯路，但是大多数，在第一层技术环节就输了。
看上去像是另外一场寒冬。



## 相关法律法规

[中华人民共和国劳动法](http://www.npc.gov.cn/wxzl/gongbao/2000-12/05/content_5004622.htm)
* 第四十三条 
* 第四十四条
* 第四十五条


[中华人民共和国劳动合同法](http://www.gov.cn/flfg/2007-06/29/content_669394.htm)

* 第四十七条
* 第四十八条
* 第六十二条
* 第八十七条

[中华人民共和国职工带薪年休假条例](http://www.gov.cn/flfg/2007-12/16/content_835527.htm)

* 第三条 年假计算天数
* 第五条 支付年假是工资报酬


[中华人民共和国合同法](http://www.npc.gov.cn/wxzl/wxzl/2000-12/06/content_4732.htm)

* 第一百二十四条 合同法适用于劳动合同


[中华人民共和国著作权法](http://www.npc.gov.cn/npc/xinwen/2010-02/26/content_1544852.htm)

* 第四条  著作权不能违法
* 第十六条 职务作品 以及 物质奖励依据


## 详细法律法规

### 涉及到权利与义务履行的条款

《中华人民共和国劳动法》
第一条　为了完善劳动合同制度，明确劳动合同双方当事人的权利和义务，保护劳动者的合法权益，构建和发展和谐稳定的劳动关系，制定本法。


《中华人民共和国合同法》
第十条    当事人订立合同，有书面形式、口头形式和其他形式。
第十一条    书面形式是指合同书、信件和数据电文（包括电报、电传、传真、电子数据交换和电子邮件）等可以有形地表现所载内容的形式。
第十二条    合同的内容由当事人约定，一般包括以下条款：

        （一）当事人的名称或者姓名和住所；

        （二）标的；

        （三）数量；

        （四）质量；

        （五）价款或者报酬；

        （六）履行期限、地点和方式；

        （七）违约责任；

        （八）解决争议的方法。


第二十条    有下列情形之一的，要约失效：
 
        （三）承诺期限届满，受要约人未作出承诺；
 

### 涉及到要约需要履行的条款

《中华人民共和国合同法》

第十四条    要约是希望和他人订立合同的意思表示，该意思表示应当符合下列规定：

        （一）内容具体确定；

        （二）表明经受要约人承诺，要约人即受该意思表示约束。


第十九条    有下列情形之一的，要约不得撤销：

        （一）要约人确定了承诺期限或者以其他形式明示要约不可撤销；

        （二）受要约人有理由认为要约是不可撤销的，并已经为履行合同作了准备工作。


第二十一条    承诺是受要约人同意要约的意思表示。

第四十五条    当事人对合同的效力可以约定附条件。附生效条件的合同，自条件成就时生效。附解除条件的合同，自条件成就时失效。
当事人为自己的利益不正当地阻止条件成就的，视为条件已成就；不正当地促成条件成就的，视为条件不成就。


第六十二条    当事人就有关合同内容约定不明确，依照本法第六十一条的规定仍不能确定的，适用下列规定：

        （四）履行期限不明确的，债务人可以随时履行，债权人也可以随时要求履行，但应当给对方必要的准备时间。


第一百零八条    当事人一方明确表示或者以自己的行为表明不履行合同义务的，对方可以在履行期限届满之前要求其承担违约责任。



### 涉及到合同不规范的条款

《中华人民共和国劳动合同法》
第七条　用人单位自用工之日起即与劳动者建立劳动关系。用人单位应当建立职工名册备查。

第十条　建立劳动关系，应当订立书面劳动合同。
    已建立劳动关系，未同时订立书面劳动合同的，应当自用工之日起一个月内订立书面劳动合同。
    用人单位与劳动者在用工前订立劳动合同的，劳动关系自用工之日起建立。


第四十八条　用人单位违反本法规定解除或者终止劳动合同，劳动者要求继续履行劳动合同的，用人单位应当继续履行；
劳动者不要求继续履行劳动合同或者劳动合同已经不能继续履行的，用人单位应当依照本法第八十七条规定支付赔偿金。


### 涉及到合同欺诈的条款

《中华人民共和国劳动合同法》
 第二十六条　下列劳动合同无效或者部分无效：

    （一）以欺诈、胁迫的手段或者乘人之危，使对方在违背真实意思的情况下订立或者变更劳动合同的；

    （二）用人单位免除自己的法定责任、排除劳动者权利的；

    （三）违反法律、行政法规强制性规定的。

对劳动合同的无效或者部分无效有争议的，由劳动争议仲裁机构或者人民法院确认。


根据《中华人名共和国劳动法》第十八条
第十八条    下列劳动合同无效：

        （一）违反法律、行政法规的劳动合同；

        （二）采取欺诈、威胁等手段订立的劳动合同。


《中华人民共和国合同法》

第四十二条    当事人在订立合同过程中有下列情形之一，给对方造成损失的，应当承担损害赔偿责任：

        （一）假借订立合同，恶意进行磋商；

        （二）故意隐瞒与订立合同有关的重要事实或者提供虚假情况；

        （三）有其他违背诚实信用原则的行为。

第五十四条    下列合同，当事人一方有权请求人民法院或者仲裁机构变更或者撤销：

        （一）因重大误解订立的；

        （二）在订立合同时显失公平的。

一方以欺诈、胁迫的手段或者乘人之危，使对方在违背真实意思的情况下订立的合同，受损害方有权请求人民法院或者仲裁机构变更或者撤销。
当事人请求变更的，人民法院或者仲裁机构不得撤销。

