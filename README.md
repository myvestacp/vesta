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

Features of myVesta
==================================================

+ Support for Debian 10 (previous Debian releases are also supported, but Debian 10 is recommended)

+ [nginx templates](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/rate-limit-tpl/install_rate_limit_tpl.sh) that can prevent denial-of-service on your server

+ [Support for multi-PHP versions](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/multi-php-install.sh)

+ You can limit the maximum number of sent emails (per hour) [per mail account](https://github.com/myvesta/vesta/blob/master/install/debian/10/exim/exim4.conf.template#L109-L110) and [per hosting account](https://github.com/myvesta/vesta/blob/master/install/debian/10/exim/exim4.conf.template#L72-L73), preventing hijacking of email accounts and preventing PHP malware scripts to send spam.

+ You can see [what PHP scripts are sending emails](https://github.com/myvesta/vesta/blob/master/install/debian/10/php/php7.3-dedi.patch#L50), when and to whom

+ You can completely "lock" myVesta so it can be accessed only via **secret URL**, for example https://serverhost:8083/?MY-SECRET-URL
    + During installation you will be asked to choose a secret URL for your hosting panel
    + Literally no PHP scripts will be alive on your hosting panel (won't be able to get executed), unless you access the hosting panel with secret URL parameter. Thus, when it happens that, let's say, some zero-day exploit pops up - attackers won't be able to access it without knowing your secret URL - PHP scripts from VestaCP will be simply dead - no one will be able to interact with your panel unless they have the secret URL.
    + You can see for yourself how this mechanism was built by looking at:
      + https://github.com/myvesta/vesta/blob/master/src/deb/for-download/php/php.ini#L496
      + https://github.com/myvesta/vesta/blob/master/web/inc/secure_login.php
    + If you didn't set the secret URL during installation, you can do it anytime. Just execute in shell:
    + `echo "<?php \$login_url='MY-SECRET-URL';" > /usr/local/vesta/web/inc/login_url.php`

+ We [disabled dangerous PHP functions](https://github.com/myvesta/vesta/blob/master/install/debian/10/php/php7.3-dedi.patch#L9) in php.ini, so even if, for example, your customer's CMS gets compromised, hacker will not be able to execute shell scripts from within PHP.

+ Apache is fully switched to mpm_event mode, while PHP is running in PHP-FPM mode, which is the most stable PHP-stack solution
    + OPCache is turned on by default

+ Auto-generating LetsEncrypt SSL for server hostname (signed SSL for Vesta 8083 port, for dovecot (IMAP & POP3) and for Exim (SMTP))

+ You can change Vesta port during installation or later using one command line: **v-change-vesta-port [number]**

+ ClamAV is configured to block zip/rar/7z archives that contains executable files (just like GMail)

+ Backup will run with lowest priority (to avoid load on server), and can be configured to run only by night (and to stop on the morning and continue next night)

+ You can compile Vesta binaries by yourself - https://github.com/myvesta/vesta/blob/master/src/deb/vesta_compile.sh
    + You can even create your own APT repository in a minute
    + We are using latest nginx version for vesta-nginx package
    + With your own APT infrastructure you can take security of Vesta-installer infrastructure in your own hands. You will have full control of your Vesta code (this way you can rest assured that there's 0% chance that you'll install malicious packages from repositories that may get hacked)
    + Binaries that you compile are 100% compatible with official VestaCP from vestacp.com, so you can run official VestaCP code with your own binaries (in case you don't want the source code from this fork)

Useful tools
==================================================

+ [Script that will convert Vesta to myVesta](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/convert-vesta-to-myvesta.sh)

+ [Wordpress installer in one second](https://github.com/myvesta/vesta/blob/master/bin/v-install-wordpress)

+ [Script for importing cPanel backups to Vesta](https://forum.myvestacp.com/viewtopic.php?f=24&t=63)

+ [Cloning script that will copy the whole site from one domain to another (sub)domain](https://github.com/myvesta/vesta/blob/master/bin/v-clone-website)

+ [Script that will migrate your site from http to https, replacing http to https URLs in database](https://github.com/myvesta/vesta/blob/master/bin/v-migrate-site-to-https)

+ [Script that will install multiple PHP versions on your server](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/multi-php-install.sh)

+ [Script that will install nginx templates that can prevent denial-of-service on your server](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/rate-limit-tpl/install_rate_limit_tpl.sh)

+ [Official Vesta Softaculous installer](https://github.com/myvesta/vesta/blob/master/src/deb/for-download/tools/install-softaculous.sh)


How to install
----------------------------
Download the installation script:
```bash
curl -O http://c.myvestacp.com/vst-install-debian.sh
```
Then run it:
```bash
bash vst-install-debian.sh
```

About VestaCP
==================================================

* [Vesta](https://vestacp.com/) is an open source hosting control panel.
* Vesta has a clean and focused interface without clutter.
* Vesta has the latest of very innovative technologies.

Special thanks to vestacp.com and Serghey Rodin for open-source VestaCP project

License
----------------------------
Vesta is licensed under  [GPL v3 ](https://github.com/serghey-rodin/vesta/blob/master/LICENSE) license

