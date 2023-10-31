#实验1:双绞线制作
我们按交叉线接法或者直连线接法都可以 
https://blog.csdn.net/weixin_44703147/article/details/102837506

https://zh.wikipedia.org/zh-hans/TIA/EIA-568

#实验2：熟悉交换机配置

#实验3：跨交换机同一vlan通信
这个是本次实验课的ppt
这个PPT的内容和里面的命令我都对过了，如果有什么小问题的话也跟我说。
如果有事没能及时来或者没听清楚 实验要求什么的，不用害怕，刚刚讲过的问题也可以问易老师和我，有问题要及时解决）

我们本周跨交换机，实验预期结果是同一vlan 主机可以互相 ping 成功，不同vlan 主机不可以。
所有主机都是同一网络号不同主机号。如果主机不够最少可以用三台（两台同一vlan，一台不同vlan）

#4 rstp
我们本周配置 Rstp
##实验步骤
配置两台交换机的TAG VLAN端口（trunk 口）
两台交换机 配置快速生成树  两台交换机上开启生成树协议 并指定生成树协议类型为RSTP
按照拓扑图连接对应交换机的端口
分别查看两台交换机的生成树信息。
分别查看两台交换机的  Tag Vlan 端口（trunk 口）状态
我们不用 ping ,我们拔下原来 forwarding 的端口所接网线，观察原来 discarding 的端口变为 forwarding

show spanning-tree 看 RootPort: . RootPort: 0 的是根交换机，没有根口,根交换机的每个端口都是指定端口 PortRole : designatedPort 。PortState : forwarding

show spanning-tree interface  --   
看 PortState : forwarding / discarding 和 PortRole :  rootPort / designedPort / alternatePort 。
根端口是非根交换机去往根交换机路径最优的端口，一个非根交换机上最多只有一个根端口。  PortRole : rootPort   PortState : forwarding ，一个 PortRole : alternatePort 端口  PortState : discarding

观察好了之后，我们拔下原来 非根交换机 PortState : forwarding 的  PortRole : rootPort 端口所接网线，观察原来 PortState : discarding 的 PortRole : alternatePort  端口变为 PortState : forwarding 的  PortRole : rootPort 端口。

show span interface  --   看 PortState : forwarding / discarding 和 PortRole :  rootPort / designedPort。非根交换机有一个根口  forwarding ，每一个直连有一个 指定端口  forwarding ， 一个 非指定端口  discarding
我们拔下原来 forwarding 的端口所接网线，观察原来 discarding 的端口变为 forwarding

我们的这个实验可以不用设置交换机优先级，就默认的优先级就可以。
注意：使用下三台交换机的要把端口改成 gi 0/

en
conf
interface range fastethernet 1/1-2 
switchport mode trunk 
exit
spanning-tree
spanning-tree mode rstp

show spanning-tree
show spanning-tree interface fa 1/1
show spanning-tree interface fa 1/2

en
conf
interface range gi 0/1-2 
switchport mode trunk 
exit
spanning-tree
spanning-tree mode rstp

show spanning-tree
show spanning-tree interface gi 0/1
show spanning-tree interface gi 0/2

我刚刚发的PPT和实验报告的内容和命令我都对过了，如果有什么小问题的话也跟我说。
