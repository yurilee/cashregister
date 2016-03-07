# cashregister
>ThoughtWorks代码题目：收银机
>测试脚本：src/main/liyy/exercise/Main.groovy

# 一、需求描述
原文见：[ThoughtWorks代码题目](https://jinshuju.net/f/n0ddSe?from=groupmessage&isappinstalled=0)

>商店里进行购物结算时会使用收银机系统，这台收银机会在结算时根据客户的购物车中的商品和商店正在进行的优惠活动进行结算和打印购物小票。
>已知商品信息包含：名称，数量单位，单价，类别和条形码（伪）。 
>已知我们可以对收银机进行设置，使之支持各种优惠。

我们需要实现一个名为打印小票的小模块，收银机会将输入的数据转换成一个JSON数据然后一次性传给我们这个小模块，我们将从控制台中输出结算清单的文本。

####输入格式（样例）：
>[
    'ITEM000001',
    'ITEM000001',
    'ITEM000001',
    'ITEM000001',
    'ITEM000001',
    'ITEM000003-2',
    'ITEM000005',
    'ITEM000005',
    'ITEM000005'
]

>其中对'ITEM000003-2'来说,"-"之前的是标准的条形码,"-"之后的是数量。 
>当我们购买需要称量的物品的时候,由称量的机器生成此类条形码,收银机负责识别生成小票。

####该商店正在对部分商品进行“买二赠一”的优惠活动和对部分商品进行95折的优惠活动
>其中：
“买二赠一”是指，每当买进两个商品，就可以免费再买一个相同商品。
“95折”是指，在计算小计的时候按单价的95%计算每个商品。
每一种优惠都详细标记了哪些条形码对应的商品可以享受此优惠。
店员设置，当“95折”和“买二赠一”发生冲突的时候，也就是一款商品既符合享受“买二赠一”优惠的条件，又符合享受“95折”优惠的条件时，只享受“买二赠一”优惠。

##要求写代码支持上述的功能，并根据输入和设置的不同，输出下列小票。

####小票内容及格式（样例）：

1. 当购买的商品中，有符合“买二赠一”优惠条件的商品时：
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.00(元)，小计：6.00(元)
名称：羽毛球，数量：5个，单价：1.00(元)，小计：4.00(元)
名称：苹果，数量：2斤，单价：5.50(元)，小计：11.00(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
买二赠一商品：
名称：可口可乐，数量：1瓶
名称：羽毛球，数量：1个
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：21.00(元)
节省：4.00(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

2. 当购买的商品中，没有符合“买二赠一”优惠条件的商品时：
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.00(元)，小计：9.00(元)
名称：羽毛球，数量：5个，单价：1.00(元)，小计：5.00(元)
名称：苹果，数量：2斤，单价：5.50(元)，小计：11.00(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：25.00(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

3. 当购买的商品中，有符合“95折”优惠条件的商品时
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.00(元)，小计：9.00(元)
名称：羽毛球，数量：5个，单价：1.00(元)，小计：5.00(元)
名称：苹果，数量：2斤，单价：5.50(元)，小计：10.45(元)，节省0.55(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：24.45(元)
节省：0.55(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

4. 当购买的商品中，有符合“95折”优惠条件的商品，又有符合“买二赠一”优惠条件的商品时
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.00(元)，小计：6.00(元)
名称：羽毛球，数量：6个，单价：1.00(元)，小计：4.00(元)
名称：苹果，数量：2斤，单价：5.50(元)，小计：10.45(元)，节省0.55(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
买二赠一商品：
名称：可口可乐，数量：1瓶
名称：羽毛球，数量：2个
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：20.45(元)
节省：5.55(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

# 二、需求分析
>约定：
  - 商品的编号和名称，依次为[ITEM000001:可口可乐、ITEM000002:羽毛球、ITEM000003:苹果]
  - 购物清单中，以商品编号排序

####小票的内容分为三大类：
1. 购物清单。商品显示的信息与活动有关，比如，打折，显示“节省0.55(元)”
2. “买二赠一”赠送的商品
3. 总计[节省]
####有两种促销活动：
1. 打95折
2. 买二赠一
####有四个场景（彼此之间没有关系），每个场景中的商品参与的活动，从实际输出的小票分析得出
  - 有符合“买二赠一”优惠条件的商品
      + 优惠活动类型："买二赠一"，
        享受优惠的产品及个数：ITEM000001-3，ITEM000002-5
      + 没有享受优惠活动的产品及个数：ITEM000003-2
  - 没有满足任何一种优惠活动的商品
      + ITEM000001-3，ITEM000002-5，ITEM000003-2
  - 有符合“95折”优惠条件的商品
     + 优惠活动类型："95折",
       享受优惠的产品及个数：ITEM000003-2
     + 没有享受优惠活动的产品及个数：ITEM000001-3，ITEM000002-5
  - 有些商品同时满足两种优惠活动的条件
     + 优惠活动类型："95折",享受优惠的产品及个数：ITEM000001-3，ITEM000002-6，ITEM000003-2;
       优惠活动类型："买二赠一",享受优惠的产品及个数：ITEM000001-3，ITEM000002-6
 ***注：可以确定参与“买二赠一”活动的产品是ITEM000001、ITEM000002，但，参与“95折”活动的产品，仅能确定ITEM000003。其他两种产品是否参与“95折”活动，都可以！即，演示数据有4情况（2的全排列）。现在，仅选择其中的一种情况***
       

# 三、代码最终输出的内容
有符合“买二赠一”优惠条件的商品
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.0(元)，小计：6.0(元)
名称：羽毛球，数量：5个，单价：1.0(元)，小计：4.0(元)
名称：苹果，数量：2斤，单价：5.5(元)，小计：11.0(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
买二赠一商品：
名称：可口可乐，数量：1瓶
名称：羽毛球，数量：1个
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：21.0(元)
节省：4.0(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
没有满足任何一种优惠活动的商品
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.0(元)，小计：9.0(元)
名称：羽毛球，数量：5个，单价：1.0(元)，小计：5.0(元)
名称：苹果，数量：2斤，单价：5.5(元)，小计：11.0(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：25.0(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
有符合“95折”优惠条件的商品
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.0(元)，小计：9.0(元)
名称：羽毛球，数量：5个，单价：1.0(元)，小计：5.0(元)
名称：苹果，数量：2斤，单价：5.5(元)，小计：10.45(元)，节省0.55(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：24.45(元)
节省：0.55(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
有些商品同时满足两种优惠活动的条件
\*\*\*<没钱赚商店>购物清单\*\*\*
名称：可口可乐，数量：3瓶，单价：3.0(元)，小计：6.0(元)
名称：羽毛球，数量：6个，单价：1.0(元)，小计：4.0(元)
名称：苹果，数量：2斤，单价：5.5(元)，小计：10.45(元)，节省0.55(元)
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
买二赠一商品：
名称：可口可乐，数量：1瓶
名称：羽毛球，数量：2个
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
总计：20.45(元)
节省：5.55(元)
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
