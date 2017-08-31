# bluemix-dev

このVagrantfile は Bluemix のアプリを開発するための仮想マシンをVagrant のLinux上に作るものです。


## このファイルの内容


以下のプログラム言語やツールが動作する環境が構築できます。

* Bluemix CLI コマンド (bxコマンド)
* kubectl k8sの管理用コマンド 
* ruby,php,python,node.js をビルドするために必要なaptモジュール導入
* clpplus を実行するために必要なJRE環境 (clpplusは入っていません)
* ruby バージョン切り替えとビルドツール rbenv,ruby-build
* PHP バージョン切り替えとビルドツール phpenv,php-build
* Python バージョン切り替えツール pyenv
* JavaScript バージョン切り替えとビルドツール ndenv,node-build
* Docker-CE Docker実行環境 (dockerコマンド)
* Xserver への接続 xeyesなど確認用アプリとライブラリ 


## 前提条件

* Windows, Mac のパソコンに、Vagrant + VirtualBox の環境が導入されている事
* VirtualBox の仮想マシンに 1GのRAMが割り当てられる事


## 利用方法

開発用のパソコンに、git クローンします。

~~~
git clone https://github.com/takara9/bluemix-dev
~~~

gitで作成したフォルダへ移動します。

~~~
cd bluemix-dev
~~~

仮想マシンを起動します。

~~~
vagrant up
~~~

起動したらログインします。

~~~
imac:bluemix-dev maho$ vagrant ssh
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-125-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Mon Jul 31 04:41:28 UTC 2017

  System load:  0.62              Processes:           84
  Usage of /:   3.6% of 39.34GB   Users logged in:     0
  Memory usage: 12%               IP address for eth0: 10.0.2.15
  Swap usage:   0%

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

New release '16.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
~~~



## Bluemix へのログイン

Bluemix CLIがインストールされるので、すぐに自分のアカウントでBluemixへログインできます。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ bx login -a api.ng.bluemix.net -u [your IBMid] -p 'your password' -s dev
~~~

## Bluemix コンテナサービスのプラグイン

この Vagrantfile では Docker-CE を導入していますので、Bluemix CLI のプラグインをインストールするだけで、Bluemixのk8sコンテナを管理できます。
重要: bx ic で利用するコンテナサービスは終了しました。


## 便利なBluemix CLI プラグイン

その他のプラグインも導入しておくと便利です。

#### bx plugin install container-service -r Bluemix

Bluemix の Kubernetes クラスターを操作するためのプラグイン

#### bx plugin install container-registry -r Bluemix

レジストリーと、アカウント用のレジストリー・リソースを管理するためのプラグイン

上記のプラグインをインストールした後に、リストを表示すると次の様になります。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ bx plugin list
Listing installed plug-ins...

Plugin Name          Version   
container-registry   0.1.171   
container-service    0.1.292
~~~



## 開発環境

### PHPのバージョンの選定とビルド

PHPのビルドに必要なモジュールは全て導入済みなので、次のコマンドを実行するだけで、指定したバージョンのPHPが利用できます。

導入可能なphpのバージョンのリスト取得

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ phpenv install -l
Available versions:
  5.2.17
  5.3.2
  5.3.3
  5.3.6
  5.3.8
  5.3.9
<以下省略>
~~~
バージョンを選んでビルド＆インストールは次のコマンドです。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ phpenv install 5.6.31
[Info]: Loaded extension plugin
[Info]: Loaded apc Plugin.
[Info]: Loaded composer Plugin.
[Info]: Loaded github Plugin.
[Info]: Loaded uprofiler Plugin.
[Info]: Loaded xdebug Plugin.
[Info]: Loaded xhprof Plugin.
[Info]: Loaded zendopcache Plugin.
[Info]: php.ini-production gets used as php.ini
[Info]: Building 5.6.31 into /home/vagrant/.phpenv/versions/5.6.31
[Downloading]: https://secure.php.net/distributions/php-5.6.31.tar.bz2
[Preparing]: /tmp/php-build/source/5.6.31
[Compiling]: /tmp/php-build/source/5.6.31
[xdebug]: Installing version 2.5.5
[xdebug]: Compiling xdebug in /tmp/php-build/source/xdebug-2.5.5
[xdebug]: Installing xdebug configuration in /home/vagrant/.phpenv/versions/5.6.31/etc/conf.d/xdebug.ini
[xdebug]: Cleaning up.
[Info]: Enabling Opcache...
[Info]: Done
[Info]: The Log File is not empty, but the Build did not fail. Maybe just warnings got logged. You can review the log in /tmp/php-build.5.6.31.20170731044950.log
[Success]: Built 5.6.31 successfully.
~~~

PHP動作確認

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ phpenv global 5.6.31
5.6.31
vagrant@vagrant-ubuntu-trusty-64:~$ php -v
PHP 5.6.31 (cli) (built: Jul 31 2017 04:57:46) 
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
    with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2016, by Zend Technologies
    with Xdebug v2.5.5, Copyright (c) 2002-2017, by Derick Rethans
~~~



### Rubyの場合

コマンドはrbenv でオプションはphpenvと同じです。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ rbenv install 2.4.1
Downloading ruby-2.4.1.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.bz2
Installing ruby-2.4.1...
Installed ruby-2.4.1 to /home/vagrant/.rbenv/versions/2.4.1
vagrant@vagrant-ubuntu-trusty-64:~$ rbenv global 2.4.1
~~~

ruby のバージョンを確認してみます。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ ruby --version
ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-linux]
~~~



### Pythonの場合

他と同じです。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ pyenv install 3.6.2
Downloading Python-3.6.2.tar.xz...
-> https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tar.xz
Installing Python-3.6.2...
Installed Python-3.6.2 to /home/vagrant/.pyenv/versions/3.6.2

vagrant@vagrant-ubuntu-trusty-64:~$ pyenv global 3.6.2
~~~

pyenvは、再ログインしないと有効にならないらしい。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ python --version
Python 3.6.2
~~~



### Node.jsの場合
PHPなどと比べると、異常に切り替えが早いです。バイナリを展開しているだけの様な速さです。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ ndenv install v5.9.1
Downloading node-v5.9.1-linux-x64.tar.gz...
-> https://nodejs.org/dist/v5.9.1/node-v5.9.1-linux-x64.tar.gz
Installing node-v5.9.1-linux-x64...
Installed node-v5.9.1-linux-x64 to /home/vagrant/.ndenv/versions/v5.9.1
~~~
バージョンの切り替え確認です。

~~~
vagrant@vagrant-ubuntu-trusty-64:~$ node -v
v5.9.1
~~~


## 停止と削除

仮想マシンの停止で、ホスト・マシン(Mac or Windows)のメモリが解放されます。

~~~
vagrant halt
~~~

仮想マシンの削除で、ホスト・マシン(Mac or Windows)のディスクから仮想マシンのディスク・イメージが削除されます。

~~~
vagrant destroy
~~~




