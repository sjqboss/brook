# Brook端口转发一键脚本再次修改版
Brook 端口转发 一键管理脚本再次修改版 基于逗比，yulewang版本修改而来。

去掉了更新之类的功能。删除iptables端口放行规则，更换为全允许。

解决之前脚本不支持CNAME的问题，将DDNS监测周期更换为1min。

-----------------------------------------------------------------------------

## 写在前面
一提到转发，大家最先想到的应该都是内核态方式，最常用的也就是iptables。因为这种实现方式对系统资源的消耗都比较少，性能比起软件转发更有优势。

**为什么会有此脚本**

之前我也是力推iptables，但在DDNS实现上较为复杂。有大佬写了很好用的脚本，但规则一多，要么删除不完全，要么删掉一大堆本来不应该删掉的规则。使用过程中难免会在调试阶段删除/添加规则，删除某一条后发现突然少了20多条规则，这样的事应该都不想在正常使用中遇到吧。正好我看到逗比的Brook转发脚本被大佬修改支持DDNS，试了一下发现CNAME无法使用，所以结合了其他脚本的域名拿IP方法，构成了此脚本(拼接怪

**此脚本优势**

类似于nftables，Brook的转发内容都是写在文件内(.conf)，方便在各机器中同步规则，不会出现删除多，删除不干净。直接编辑配置文件保存后重启Brook即可重新加载规则。

**此脚本劣势**

首先即是无法转发端口段，如要转发端口段则请避免使用此转发脚本。其次对系统的资源(CPU,RAM)有一定占用，当然比隧道还是好多了xx

## 使用方法
```shell
wget -qO brook-pf-mod.sh https://raw.githubusercontent.com/sjqboss/brook/master/brook-pf-mod.sh && chmod +x brook-pf-mod.sh && bash brook-pf-mod.sh
```
执行结果：
```
  Brook 端口转发 一键管理脚本修改版(DDNS支持) [v1.0.1]  
  1. 安装 Brook
  2. 卸载 Brook
————————————
  3. 启动 Brook
  4. 停止 Brook
  5. 重启 Brook
————————————
  6. 设置 Brook 端口转发
  7. 查看 Brook 端口转发
  8. 查看 Brook 日志
  9. 监控 Brook 运行状态(如果使用DDNS必须打开)
 ————————————
 10. 安装CNAME依赖(若添加DDNS出现异常)
 11. 安装服务脚本(执行安装Brook后请勿重复安装)
 12. iptables一键放行
————————————
 当前状态: 未安装
 请输入数字 [0-12]:
```
按1后回车即可自动安装完成。

**如需开启DDNS支持，请在安装完成后按9开启运行监控。**

## 手动安装
因国内下载github资源速度缓慢，建议国内机器使用手动安装来使用。

根据您的服务器系统版本下载对应版本。(v20200801)
```
https://github.com/txthinking/brook/releases
```
创建并将下载的二进制文件上传到 **/usr/local/brook-pf** 目录

在目录建立 **brook.conf** 和 **brook.log** 两个文件，不要写任何内容。(除非你明白你在做什么)

保证目录下有3个文件后赋予权限：
```
chmod +x /usr/local/brook-pf
```
接下来进入脚本依次执行第10，11，12项。

在执行第11项时可能会出现无法下载的问题，请根据系统类型下载项目内的**brook-pf_debian** 或 **brook-pf_centos**

下载完后改名为 **brook-pf**，上传至 **/etc/init.d/** ，然后执行：
```
chmod +x /etc/init.d/brook-pf
```
Enjoy~
