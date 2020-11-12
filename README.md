新闻
==================================================

我们终于有了正常的主页：
- https://www.myvestacp.org/

论坛：
- https://forum.myvestacp.org/

知识库：
- https://wiki.myvestacp.org/

更新日志：
- https://github.com/myvestacp/vesta/blob/master/Changelog.md

myVesta控制面板
==================================================

* myVesta是[VestaCP](https://vestacp.com/)的一个分叉
* 专注于安全性和稳定性
* 因此，仅支持Debian-仅关注一个生态系统-不浪费精力与其他Linux发行版兼容
* 但是，它将始终与官方VestaCP提交同步
* VestaCP商业插件仅可在[vestacp.com](https://vestacp.com/)官方网站上购买-我们不会收取他们的收入，因为出于金钱原因我们不作此准备。取而代之的是，我们会在考虑开源的情况下进行此操作-增强安全性并构建新功能，而不会与VestaCP的正式发布周期挂钩，也不会影响或严重偏离VestaCP的计划开发里程碑
* 牢记先前的观点，为此分叉（myVesta）构建的所有功能将通过拉取请求提供给官方VestaCP。

myVesta的功能
==================================================

+ 支持Debian 10（也支持以前的Debian版本，但建议使用Debian 10）

+ [nginx模板](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/rate-limit-tpl/install_rate_limit_tpl.sh)可以防止服务器上的拒绝服务

+ [支持多PHP版本](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/multi-php-install.sh)

+ 您可以限制[每个邮件帐户](https://github.com/myvestacp/vesta/blob/master/install/debian/10/exim/exim4.conf.template#L109-L110)和[每个托管帐户](https://github.com/myvestacp/vesta/blob/master/install/debian/10/exim/exim4.conf.template#L72-L73)的最大发送电子邮件数量（每小时），，从而防止电子邮件帐户被劫持并防止PHP恶意软件脚本发送垃圾邮件。

+ 您可以查看[哪些PHP脚本](https://github.com/myvestacp/vesta/blob/master/install/debian/10/php/php7.3-dedi.patch#L50)向谁发送电子邮件，何时发送给谁。

+ 您可以完全“锁定” myVesta，以便只能通过秘密URL进行访问，例如：https://serverhost:8083/?MY-SECRET-URL
    + 在安装过程中，系统将要求您为主机面板选择一个秘密URL。
    + 从字面上看，除非您使用秘密URL参数访问托管面板，否则任何PHP脚本都不会在您的托管面板上运行（无法执行）。因此，当发生这种情况时，例如，出现了一些零日漏洞利用-攻击者将无法在不知道您的秘密URL的情况下访问它-VestaCP的PHP脚本将完全死亡-没有人可以与之交互您的面板，除非他们有秘密URL。
    + 您可以通过查看以下内容亲自了解此机制的构建方式：
      + https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/php/php.ini#L496
      + https://github.com/myvestacp/vesta/blob/master/web/inc/secure_login.php
    + 如果您在安装过程中未设置秘密URL，则可以随时进行设置。只需在shell中执行：
    + `echo "<?php \$login_url='MY-SECRET-URL';" > /usr/local/vesta/web/inc/login_url.php`

+ 我们在php.ini中[禁用了危险的PHP函数](https://github.com/myvestacp/vesta/blob/master/install/debian/10/php/php7.3-dedi.patch#L9)，因此，即使，例如，您客户的CMS受到威胁，黑客也将无法从PHP内部执行Shell脚本。

+ Apache已完全切换到mpm_event模式，而PHP在PHP-FPM模式下运行，这是最稳定的PHP堆栈解决方案。
    + OPCache默认情况下处于打开状态

+ 自动生成用于服务器主机名的LetsEncrypt SSL（用于Vesta 8083端口，用于dovecot（IMAP＆POP3）和用于Exim（SMTP）的签名SSL）

+ 您可以在安装过程中或以后使用一个命令行更改Vesta端口：**v-change-vesta-port [number]**

+ ClamAV配置为阻止包含可执行文件的zip/rar/7z存档（就像GMail一样）

+ Backup备份将以最低优先级运行（以避免服务器负载），并且可以配置为仅在晚上运行（并在早上停止并在第二天晚上继续）

+ 您可以自己编译Vesta二进制文件 - https://github.com/myvestacp/vesta/blob/master/src/deb/vesta_compile.sh
    + 您甚至可以在一分钟内创建自己的APT存储库。
    + 我们正在将最新的Nginx版本用于vesta-nginx软件包。
    + 使用您自己的APT基础架构，您可以自己掌握Vesta安装程序基础架构的安全性。您将完全控制您的Vesta代码（通过这种方式，您可以放心，您有0％的可能性会从可能会被黑客入侵的存储库中安装恶意软件包）。
    + 您编译的二进制文件与vestacp.com的官方VestaCP 100％兼容，因此您可以使用自己的二进制文件运行官方的VestaCP代码（以防您不希望此派生的源代码）

有用的工具
==================================================

+ [将Vesta转换为myVesta的脚本](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/convert-vesta-to-myvesta.sh)

+ [一秒钟即可完成WordPress的安装程序](https://github.com/myvestacp/vesta/blob/master/bin/v-install-wordpress)

+ [将cPanel备份导入到Vesta的脚本](https://forum.myvestacp.org/viewtopic.php?f=23&t=48)

+ [克隆脚本会将整个站点从一个（子）域复制到另一个（子）域](https://github.com/myvestacp/vesta/blob/master/bin/v-clone-website)

+ [将您的网站从http迁移到https的脚本，将数据库中的http替换为https URL](https://github.com/myvestacp/vesta/blob/master/bin/v-migrate-site-to-https)

+ [将在您的服务器上安装多个PHP版本的脚本](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/multi-php-install.sh)

+ [脚本将安装nginx模板，以防止服务器上的拒绝服务](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/rate-limit-tpl/install_rate_limit_tpl.sh)

+ [Vesta Softaculous官方安装程序](https://github.com/myvestacp/vesta/blob/master/src/deb/for-download/tools/install-softaculous.sh)


如何安装
----------------------------
下载安装脚本：
```bash
curl -O http://c.myvestacp.org/vst-install-debian.sh
```
然后运行它：
```bash
bash vst-install-debian.sh
```

关于VestaCP
==================================================

* [Vesta](https://vestacp.com/)是一个开源的托管控制面板。
* Vesta具有干净整洁的界面，不会造成混乱。
* Vesta拥有最新的非常创新的技术。

特别感谢vestacp.com和Serghey Rodin的开源VestaCP项目

License
----------------------------
Vesta is licensed under  [GPL v3 ](https://github.com/serghey-rodin/vesta/blob/master/LICENSE) license

