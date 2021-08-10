# caddy_reverse_proxy_grpc_CDN 
caddy反代gRPC协议及上CDN的配置方法  
  
最近项目必须需使用caddy进行反代grpc协议，在网上研究很久，发现即使是caddy自己的社区都没有一个明确的配置说明，  
不得不自己进行反复试验，最后终于研究出来  

这次使用的是caddy v2.4.3版linux amd4，https://github.com/caddyserver/caddy/releases 这里下载
具体的配置文件举例如下：  
  
#nano /etc/caddy/Caddyfile  
  
写入以下内容，可以自动签证书，假设grpc的serviceName是abcd，语句reverse_proxy /abcd/*是固定格式，千万不要乱改  
 
www.youdomain.com {  
root * /var/www  
file_server  
reverse_proxy /abcd/* 127.0.0.1:12345 {  
flush_interval -1  
transport http {  
versions h2c  
}  
}  
}  
 
保存并退出 
 
---使用CDN一定要加flush_interval -1否则CDN不通！！！---  
当然了，CF要在network选项打开grpc  
配合上面caddy配置文件的grpc反代配置，在后端被反代服务器配置文件中，关于grpc的配置要注意  
serviceName字段必须写成"abcd" ,千万不要有任何/及*符号  
  
另外在客户端配置时，grpc的路径前后都千万不要有任何/及*符号  
  
