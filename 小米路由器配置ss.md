## 准备
* 小米路由PRO（R3P)
* Xshell
* ss服务器ip、端口、密码

## 第一步：刷机ROM
* 登录miwifi.com  
* 常用设置->系统状态->手动升级至开发版2.13.65    
* 文件在小米官网下载，选择与之对应的开发版本  
* 下载界面：http://www1.miwifi.com/miwifi_download.html  
* 小米默认网段是192.168.31.1 在miwifi.com上修改成192.168.1.1  

## 第二步：SSH登录路由器
在小米官网下载 miwifi_ssh.bin 拷贝到U盘  
下载方法： http://www1.miwifi.com）开放 -> 开启SSH工具   
并获得root密码xxxxxxxx  
插入路由器 用牙签按用RESET 重启 黄色闪 松牙签 等待路由器正常启动即可  
ssh root@192.168.1.1 密码在第二步获取  

## 第三步：安装Misstar Tools
使用Xshell登录路由器  
wget http://www.misstar.com/tools/appstore/install.sh -O /tmp/install.sh && chmod +x /tmp/install.sh && /tmp/install.sh  
（如果在线安装不了，请参考：hy\sa\scripts\vpn\miwifi\misstar） 

/etc/misstar/scripts/appmanager add ss  
浏览器登录刷新192.168.1.1 会看到MT工具箱   
找到科学上网 配置 ss服务器ip、端口 使用 gfwlist模式！！ 等即可  

Gfwlist模式 只有指定的才走国际线路 (一般不需要配置) 如有必要 可更新pac.conf  
配置的域名 走1081端口，走国际线路， 有默认配置 /etc/misstar/applications/ss/config/pac.conf  

不配置：走国内线路  
ipset del gfwlist 47.106.87.113  
whitelist模式（白名单）只有指定的才走国内线路   
(一般需要配置) 如发现某网址出去绕了一圈再回来 可在这配置  

配置的ip范围， 有默认配置 /etc/misstar/applications/ss/config/chnroute.conf，走国内线路  
ipset list nogfwnet 不存在这里的ip走国际线路  

不配置：走国际线路  
例如：  
192.168.1.200/32  
192.168.1.0/24  
14.215.0.0/16  
47.106.0.0/16  
112.74.0.0/16  
120.78.0.0/16  

## 第四步：配置Hosts（非必须-一般有内网域名时使用）
使用Xshell登录路由器  
把需要内网域名服务器的hosts写入 /etc/hosts  
并在 /etc/misstar/applications/ss/config/pdnsd.conf 加入  
source {                
owner = localhost;    
file = "/etc/hosts";  
}                                                        
                                                      
rr {                                                    
name=localhost;                                       
reverse=on;                                           
a=127.0.0.1;                                          
owner=localhost;                                      
soa=localhost,root.localhost,42,86400,900,86400,86400;
}

重新连接 Misstar kx上网  

DNS加强：DNS重定向，关闭  

## 恭喜完成！  

参考：
测试DNS解析：nslookup www.baidu.com  
翻墙原理：https://icymind.com/learn-from-gfw/  
安装Misstar Tools http://bbs.xiaomi.cn/t-14328908  
openwrt解决dns污染方案https://blog.csdn.net/dai_xiangjun/article/details/50790493  

admin  
misstar.com  

卸载工具箱  
/etc/misstar/scripts/uninstall.sh  

2.0  
wget http://cloud.lifeheart.cn:188/miwifi/MT/tools/appstore/install.sh -O /tmp/install.sh && chmod +x /tmp/install.sh && /tmp/install.sh  
