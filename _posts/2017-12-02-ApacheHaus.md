
---

layout: post  

title: ApacheHaus

date: 2017-12-02 17:00:00 +0800 

categories: AS 

---

## **修改默认的路径**

在38行


Define SRVROOT "/Apache24"   

ServerRoot "${SRVROOT}"  

修改为你安装Apache的所在目录：


Define SRVROOT "D:\Android_Develop_Tools\httpd-2.4.23-x64-vc14\Apache24"   

ServerRoot "${SRVROOT}"  
如果不修改会提示错误：

httpd.exe: Syntax error on line 39 of 
D:/Android_Develop_Tools/httpd-2.4.23-x64-vc14/Apache24/conf/httpd.conf: 
ServerRoot must be a valid directory

打开解压的后的文件夹，然后进入conf目录下打开httpd.conf文件（因为我的电脑的80端口被占用了所以要修改）

## 修改端口

修改第60行

Listen 12.34.56.78:80

Listen 80  

改为8081(你可以自己随便定义，但是不要和其他的冲突即可)

Listen 12.34.56.78:80

Listen 8081  

修改第222行

ServerName localhost:80  

ServerName localhost:8081  

如果不修改会提示错误：
(OS 
10048)通常每个套接字地址(协议/网络地址/端口)只允许使用一次。  : AH00072: mak
e_sock: could not bind to 
address [::]:80
(OS 10048)通常每个套接字地址(协议/网络地址/端口)只允许使用一次。  : AH00072: 
mak
e_sock: could not bind to address 0.0.0.0:80
AH00451: no listening 
sockets available, shutting down

## 开启服务

进入到bin目录下，然后按住shift按键不放单击鼠标的右键，选择 “在此处打开命令窗口”

  httpd.exe -k install   安装服务

  httpd.exe -k start      开启服务

Stop Apache                          *httpd -k stop*

Restart Apache                      *httpd -k restart*     

Uninstall Apache Service       *httpd -k uninstall*

Test Config Syntax                 *httpd -t*

Version Details                       *httpd -V*

Command Line Options List   *httpd -h*

## 解决443端口被占了

(OS 10048)通常每个套接字地址(协议/网络地址/端口)只允许使用一次。  : 
AH00072: mak
e_sock: could not bind to address [::]:443
(OS 
10048)通常每个套接字地址(协议/网络地址/端口)只允许使用一次。  : AH00072: mak
e_sock: could not bind to 
address 0.0.0.0:443
AH00451: no listening sockets available, shutting down

打开httpd.conf, 找到加载ssl_module的那一行, 
加#号注释掉就好了:

LoadModule ssl_module modules/mod_ssl.so

现在输入httpd.exe -k 
start命令就可以了

**Markdown**

*Awesome*