load factory default  恢复出厂

# 进入配置模式
configure

# 设置主机名
set system host-name EX3300-Switch

# 设置根密码
set system root-authentication plain-text-password
New password: [输入新密码]
Retype new password: [再次输入新密码]

# 配置管理接口
set interfaces me0 unit 0 family inet address 192.168.1.2/24

# 设置默认网关
set routing-options static route 0.0.0.0/0 next-hop 192.168.1.1

# 配置DNS服务器
set system name-server 8.8.8.8

# 启用SSH服务
set system services ssh

# 定义VLAN
set vlans office vlan-id 10
set vlans guest vlan-id 20

# 配置VLAN接口
set interfaces ge-0/0/0 unit 0 family ethernet-switching vlan members office
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members guest

# 启用类似思科的Spanning Tree Portfast（在Juniper中称为Edge Port）
set protocols rstp interface ge-0/0/0 edge
set protocols rstp interface ge-0/0/1 edge

# 配置DHCP中继
set forwarding-options dhcp-relay server-group DHCP-Servers 192.168.1.10
set forwarding-options dhcp-relay active-server-group DHCP-Servers
set forwarding-options dhcp-relay group Office relays interface vlan.10
set forwarding-options dhcp-relay group Guest relays interface vlan.20

# 保存并提交配置
commit
exit
show ethernet-switching table  mac地址表


(1) 创建一个VLAN，指定VLAN名称和ID号 
set vlans vlan vlan id 10 
(2) 将交换机端口修改为access模式加入到新创建的VLAN中 
set interfaces ge-0/0/1 unit 0 family ethernet-switching port-mode access 
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members 10 
(3) 创建3层VLAN子端口，并且将子端口和VLAN关联： 
set interfaces vlan unit 10 family inet address 192.168.1.1/24 
set vlans vlan l3-interface vlan.10 //vlan子端口和VLAN对应起来

 

VLAN配置规范要求 
(1) 指定VLAN名称 
(2) 设置端口VLAN的时候指定端口为access模式 
(3) 设置interface vlan子端口的时候，unit子端口号要跟vlan id一致。


set vlans v30 vlan-id 30

set interfaces vlan unit 30 family inet address 172.30.6.1/23

set vlans v30 l3-interface vlan.30

set access address-assignment pool v30 family inet network 172.30.6.0/23
set access address-assignment pool v30 family inet dhcp-attributes router 172.30.6.1
set access address-assignment pool v30 family inet range range-1 low 172.30.6.56
set access address-assignment pool v30 family inet range range-1 high 172.30.7.254

set access address-assignment pool v30 family inet dhcp-attributes name-server 8.8.8.8


set system services dhcp-local-server group v30 interface irb.30

set system root-authentication plain-text-password 

set inter ge0/0/0 unit 0 famliy eth   vlan mem   v30
set inter ge0/0/0 unit 0 family eth  port-mode access


set system services dhcp pool 10.0.0.0/24 address-range low 10.0.0.2
set system services dhcp pool 10.0.0.0/24 address-range high 10.0.0.252
set system services dhcp pool 10.0.0.0/24 router 10.0.0.1
set interfaces vlan unit 0 family inet address 10.0.0.1/24
set vlans default l3-interface vlan.0


set system root-authentication plain-text-password
创建root密码，不创建不能保存命令
创建vlan
set vlans vlan10 vlan-id 10
set vlans vlan20 vlan-id 20

配置trunk
set interfaces ge-0/0/1 unit 0 family ethernet-switching port-mode trunk
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members [10 20]
set interfaces ge-0/0/1 unit 0 family ethernet-switching native-vlan-id 10

检查
查看接口配置
run show configuration interfaces ge0/0/1

配置access
set interfaces ge-0/0/1 unit 0 family ethernet-switching port-mode access
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members 10

管理地址
set interfaces vlan unit 10 family inet address 10.32.96.2/24

默认路由
set routing-options static route 0.0.0.0/0 next-hop 10.32.96.1
