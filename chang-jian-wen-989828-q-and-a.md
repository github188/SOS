1.如何配置永久静态路由？

第一种方法：使用root登录或者vyos用户执行sudo route add -net 192.168.20.0/24 gw 192.168.10.195

第二种方法：使用configure模式的set protocols命令

set protocols static route 192.168.20.0/24 next-hop 192.168.10.195 distance 1

添加完成后需要commit save 保存 

