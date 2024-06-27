<!-- インターネット -->
# サーバ構築手順書
---
## インターネットへの接続
### ネットワーク設定の事前確認
#### ipコマンドで現在のネットワーク設定を確認する
```
# ip addr show      -->「2: emp0s3:」から始まる行からアドレス情報を確認
```
#### ネットワーク設定は、rootからnmtuiコマンドで行う
```
# nmtui
```
### nmtuiコマンドの操作方法
#### 接続の編集画面を開くまで
オプションから「接続の編集」を選択⇒Enter⇒ 「enp0s3」を選択⇒＜編集＞に移動⇒Enterの順で操作し、接続の編集画面を開く
#### 接続の編集
IPv4設定の「手作業」を選択⇒＜表示＞をクリック⇒「アドレス」、「デフォルトゲートウェイ」に「DNSサーバ」「検索ドメイン」を入力する

設定するIDアドレス情報
|項目 |情報 |
|-- |-- |
|NIC名 |enp0s3 |
|グローバルIPアドレス |10.0.2.15/24 |
|デフォルトゲートウェイ |10.0.2.0 |
|DNSサーバ1 |10.0.2.15 |
|DNSサーバ2 |8.8.8.8 |
|検索ドメイン |r-exercise.com |

#### 接続の編集後
IPv6設定は「無視をする」を選択⇒ページ最下部の＜OK＞を選択して設定の終了する

#### 接続の有効化
オプションから「接続をアクティベートする」を選択⇒Enter⇒アクティベートを＜解除＞⇒Enter⇒もう一度＜アクティベート＞を選択⇒Enter⇒ページ最下部の戻るを選択

#### ホスト名の設定
オプションから「システムのホスト名を設定する」を選択⇒Enter⇒ホスト名を入力⇒＜OK＞を選択⇒Enter

設定するホスト名
|項目 |情報 |
|-- |-- |
|ホスト名 |h015.r-excercise.com |

#### nmtuiの終了
オプションの「終了」を選択⇒Enter

### ネットワーク設定の確認
NICの設定情報は、/etc/NetworkManager/system-connections/-<NICの名称>.nmconnectionで確認
```
# cat /etc/NetworkManager/system-connections/-emp0s3.nmconnection
```

DNSサーバと検索ドメインの情報は、/etc/resolv.confで確認
```
# cat /etc/resolv.conf
```

ホスト名は、rootからhostnameコマンドで確認
```
# hostname
```
<!-- webサーバの構築 -->
# Webサーバ構築手順

## 主な工程
1. Apacheのインストールと設定  
2. LAMP環境の構築  
3. コンテンツのダウンロード  
4. コンテンツの実行


## Apacheのインストールと設定

１.httpdパッケージがインストールされているか確認する。  
`# rpm -qi httpd`  
2.インストールされていなければ、インストールを実施する。  
`# dnf -y install httpd`  
3.Apacheを起動する  
`# systmctl start httpd`  
4.起動後、Firefoxを使用して自分のマシンのIPアドレスを指定し、テストページが開けるか確認する。  
`http://自分のマシンのIPアドレス`  
5.今後もWebサーバを使用するため、自動起動を有効にする。  
`# systemctl enable httpd`



## LAMP環境の構築
1.PHPの準備をする。AlmaLinuxではPHPのパッケージが格納されているので、phpモジュールとPDOをインストールする。  
`# dnf -y install php php-mysqlnd`  
2.ApacheにPHPモジュールを反映させるため、Apacheを再起動する。  
`# systemctl restart httpd`  
3.MariaDBをインストールする。  
`# dnf -y install mariadb-server`  
4.MariaDBの自動起動設定を有効にする。  
`# systemctl enable mariadb`  
5.MariaDBを起動する。  
`# systemctl start mariadb`  
6.MariaDBの設定をするため、MariaDBにrootでログインする。  
`# mysql -u root -p`  
7.「Enter password」の表示が出るため、何も入力せずEnterキーを押す。  
8.rootユーザのパスワードを設定する。(下記コマンドのXXXに任意のパスワードを入力)  
`alter user 'root'@'localhost' IDENTIFIED BY 'XXX'; flush privileges;`  
9.この後のコンテンツを使うため、「auth_test」の名前でデータベースを作成する。  
`create database auth_test; `  
10.データベースが作成されたか確認する。  
`show databases;`  
11.MariaDBから切断し、通常のシェルに戻る。  
`exit;`

## コンテンツのダウンロード
1.コンテンツのファイルをrootユーザのホームディレクトリにダウンロードする。  
`# wget ftp://practice.reskill.jp/pub/chap13.4.tar.gz`  
2.tarballファイルを展開する。  
`# cd /usr/local/src`  
`# tar xzvf ~/chap13.4.tar.gz`  
3.ファイルを移動する。  
`# cd auth_test`  
`# mv www/* /var/www/html`  
4.MariaDBにrootでログインする。  
`# cd ~`  
`# mysql -u root -q`  
　→パスワードを入力  
5.SQLを実行する。  
`use auth_test;`  
`source create_user_table.sql;`  
`exit;`

## コンテンツを実行する
1.ブラウザからURLを入力する。  
`http://自分のIPアドレス/login.html`  
2.「新規作成」からユーザを追加する。  
3.追加後、ログイン画面に戻り、ログインができるか確認する。

<!-- メールサーバ -->
# 2024-06-26 Linuxサーバ構築手順書

## ドキュメント作成者

岡部萌

## このドキュメントの目的、概要

当グループメンバーである
- 平野さん
- 飯島さん
- 許斐さん
- 岡部(ドキュメント作成者)

以上四名で、グループ内でのLinuxサーバ環境を、分業し円滑に構築することを目的とする。

私岡部萌が実装するメールサーバの設定内容は以下のとおりである。

- `SMTPサーバ`の起動、設定、動作確認(Postfix)

```
# dnf -y  install postfix
# systemctl start postfix
```

〈main.cfの設定変更〉
| 設定項目 | 解説 |
|---|---|
|　myhostname　|　h015.r-exercise.com　|
|　mydomain　|　r-exercise.com　|
|　myorigin　|　$myhostname　|
|　inet_interface　|　all　|
|　mydesination　|　$myhostname　|
|　mynetworks　|　10.0.2.15/24,localhost　|


- サーバが所属するネットワークセグメント内のメールはリモート配送


```
# inet_interfaces = localhost
```

- 宛先メールアドレスの＠以下がゾーンに設定したホスト名と一致する場合、ローカル配送


(宛先メールアドレスの＠以下がこの値($myhostname)と同じならローカル配送)

- 送信エラーなどによりrootに戻ってきたメールはstudent宛てに転送

```
# /etc/aliases
# root:　student
```

- `POPサーバ`の起動、設定、動作確認(Dovecot)

```
# dnf -y install  dovecot
# systemctl start dovecot
```

/etc/dovecot/dovecot.confのプロトコル(protocols)を設定 = POP3


- SMTPサーバは社内のメールのみ受信可能とする



#### 設定完了後、newaliasesコマンドを入力し、動作確認

```
# postfix check
# newaliases
```

#### 個人的に必要だと思ったもの

- メーラのインストール(Sylpheed)

```
# cd /usr/local/src
# tar xjvf ~student/ダウンロード/sylpheed-3.7.0.tar.bz2
# cd sylpheed-3.7.0
``` 

- コンパイル準備、インストール、起動

```
# dnf -y install gcc gtk2-devel
# ./configure --prefix=/usr/local/sylpheed
# make
# make install

# exit
$ /usr/local/sylpheed/bin/sylpheed &
```

- メールボックスの設定

受信サーバを「POP3」に設定　→　ユーザー名、POPサーバー名、SMTPサーバー名を入力　→　設定完了後送受信確認

## 作業完了条件

先述したグループメンバーの進捗およびエラーの有無を確認し、Github等で共有しマージしたうえでの完成とする。

## 予定作業時間

2024-06-26 13:00 ~ 2024-06-28 17:00


[手順書参考サイト](https://dev.classmethod.jp/articles/non-97-operation-manual/)

<!-- ネームサーバ -->
# ネームサーバの構築

## 方針や気を付ける点など
* 名前解決は正引きで行う
* ソフトウェアのインストールはrootに切り替えて行う
* namedが起動しないときは`/var/log/messages`を確認
* 設定を書き換えたら都度サービスの再起動を行うこと。

<table>
  <caption>作成するゾーン情報</caption>
  <thead>
    <tr>
      <th>名前</th> <th>説明</th>
    </tr>
  </thead>
  <tr>
    <td> ゾーン名 </td> <td>r-exercise.com</td>
  </tr>
  <tr>
    <td>ネームサーバのFQDN</td> <td>ns.r-exercise.com</td>
  </tr>
  <tr>
    <td>メールサーバのFQDN</td> <td>mail.r-exercise.com</td>
  </tr>
  <tr>
    <td>ホストのFQDN</td> <td>h015.r-exercise.com</td>
  </tr>
  <tr>
    <td>webサーバのFQDN</td> <td>www.r-exercise.com</td>
  </tr>
</table>

## 目次
1. [初めにやること](#初めにやること)
2. [ゾーンの定義](#ゾーンの定義)
3. [ゾーンファイルの文法チェック](#ゾーンファイルの文法チェック)
4. [正引きゾーンファイルの作成](#正引きゾーンファイルの作成)
5. [namedの起動と状態確認](#namedの起動と状態確認)
6. [名前解決の確認](#名前解決の確認)


## 1.初めにやること

### BINDがインストールされているか確認 
```
# rpm -qi named
```

### BINDをインストール 
```
# dnf -y install bind
```

### BINDの自動起動設定をする。 
```
# systemctl is-enabled named // no
# systemctl enable named 
# systemctl is-enabled named // yes
```

## 2.ネームサーバ機能の定義
`/etc/named.conf`にネームサーバ機能の定義を行う。<br>

```
options {
    listen-on port 53 { 127.0.0.1; 10.0.2.15;};
    listen-on-v6 port 53 { ::1; };
    directory "/var/named";
    dump-file   "/var/named/data/named_stats.txt";
    memstatistics-file "/var/named/data/named_mem_stats.txt";
    secroots-file "/var/named/data/named.secroots";
    recursing-file "/var/named/data/named.recursing";

    allow-query { any; };

    recursion yes;

    dnssec-validation no;
    ~
    ~
    ~
    zone "r-exercise.com" IN {
        type master;
        file "named.r-exercise.com";
    };
};
```

## 3.ゾーンファイルの文法チェック

```
# named-checkconf /etc/named.conf
```

ルートネームサーバのIPアドレスを格納するルートキャッシュはnamed.caの最新版を入手

```
dig . NS @202.12.27.33 > /var/named/named.ca
```

## 4.正引きゾーンファイルの作成
ゾーンファイルは、`/var/named/named.r-exercise.com`とする。<br>

```
$TTL 86400
r-exercise.com. IN SOA ns.r-exercise.com. root.r-exercise.com. (
    20240627
    28800
    14400
    3600000
    86400   )
r-exercise.com.     IN NS      ns.r-exercise.com.
r-exercise.com.     IN MX      10   mail.r-exercise.com.
ns.r-exercise.com.     IN A      10.0.2.15
mail.r-exercise.com.     IN A      	10.0.2.15
h015.r-exercise.com.     IN A      	10.0.2.15
www.r-exercise.com.     IN CNAME      	h015.r-exercise.com.

```

設定が終わったらnamed.r-exercise.comファイルの文法チェックを行う。
```
# named-checkzone r-exercise.com. /var/named/named.r-exercise.com
```


## 5.namedを起動させて、状態確認。

```
# systemctl start named
# systemctl status named
```

その後、namedのポートがリッスンしているかを確認

```
# ss -ntulp | grep named
```

## 6.digコマンドで名前解決できるか確認。

```
# dig h015.r-exercise.com
```
もしくは、
```
# dig 10.0.2.15
```

<!-- ssh -->

# OpenSSH SSHサーバの構築

## 方針や気を付ける点など
* 目的　→　自分の名前ユーザーからstudentにリモートログインできること
* sshが起動しているかは、`# systemctl status sshd`で確認
* 設定を書き換えたら都度サービスの再起動をすること

## 目次
1. [初めにやること](#初めにやること)
2. [一般ユーザーのアカウント作成](#一般ユーザーのアカウント作成)
3. [キーペアの作成](#キーペアの作成)
4. [公開鍵を自ホストのstudentユーザーのホームディレクトリに転送](#公開鍵を自ホストのstudentユーザーのホームディレクトリに転送)
5. [転送したカギをstudentユーザーに登録](#転送したカギをstudentユーザーに登録)
6. [ディレクトリの作成とパーミッションの設定](#ディレクトリの作成とパーミッションの設定)
7. [sshログイン](#sshログイン)
8. [UNIXパスワード認証の禁止](#UNIXパスワード認証の禁止)

## 1.初めにやること

opensshパッケージが入っているかrpmコマンドで確認。
```
# rpm -qa | grep "openss[hl]"
# rpm -qi openssh
```

/etc/ssh/sshd_configでPasswordAuthenticationがyesになっていることを確認。

## 2.一般ユーザーのアカウント作成
自分の名前のユーザーを作成する。<br>
passwdでユーザー名を指定しないと**rootのパスワードが変わってしまう**ので十分注意すること。<br>
ユーザー作成はrootで行うこと、パスワード設定はrootか設定を行うアカウントにログインして行うこと。<br>
```
# useradd YOUR_NAME
# passwd YOUR_NAME
```

## 3.キーペアの作成

作成したユーザーで公開鍵と秘密鍵のキーペアを作成する。<br>
鍵のアルゴリズムはRSAで行う。<br>
パスフレーズは任意。

```
$ ssh-keygen -t rsa
```
/home/YOUR_NAME/.sshにid_rsa.pubとid_rsaが作成されているか確認。
```
# $ ls /home/YOUR_NAME/.ssh
```

## 4.公開鍵を自ホストのstudentユーザーのホームディレクトリに転送

作成した公開鍵を自ホストのstudentユーザーのホームディレクトリに転送。
```
$ scp /home/YOUR_NAME/.ssh/id_rsa.pub student@10.0.2.15:.
```

## 5.転送したカギをstudentユーザーに登録

その後、ssh接続してみて、転送した公開鍵をstudentユーザーに登録する。
```
$ ssh student@10.0.2.15
$ cat id_rsa.pub >> .ssh/authorized_keys
```

## 6.ディレクトリの作成とパーミッションの設定

ログイン先ユーザー(student)のホームディレクトリに.sshディレクトリがある。
これがなかったらmkdirで作成。
```
$ mkdir ~/.ssh
```
その後、
authorized_keysファイルのパーミッションを600
~/.sshのパーミッションを700に設定

```
$ chmod 600 ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh
```

## 7.sshログイン

その後、ログアウトし、再度ログイン。
```
$ ssh student@10.0.2.15
```


## 8.UNIXパスワード認証の禁止


最後、サーバ側の/etc/ssh/sshd_configで、<br>
`passwordAuthentication no`にする。

<!-- firewall -->
# サーバ構築手順書
---
### ゾーンの設定
#### firewalldの有効化
firewalldサービスの起動と自動起動の設定を行う <br>
（標準でfirewalldが入っているので、インストールは行わない）
```
# systemctl enable firewalld        -->firewalldの自動起動有効化
# systemctl start firewalld         -->firewalldの起動
```

#### ゾーンの確認
指定したNIC(enp0s3)のゾーンの確認を行う <br>
（サーバとしての基本的な通信を許可するので、「public」を出力されたらOK）
```
# firewall-cmd --get-zone-of-interface=enp0s3
```

#### ゾーンの変更
指定したNIC(enp0s3)のゾーンがpublicでなければ、ゾーンを変更する
```
# firewall-cmd --zone=public --change-interface=enp0s3
```


### パケットフィルタリングの設定
#### パケットフィルタリングのルール確認
サービス設定の追加・削除を行う際はその都度ルール確認を行って、問題なく行えているか確認する
```
# firewall-cmd --list-all
```

#### サービス設定の追加
今回用いるサービス(dns, smtp, ssh, http)を追加して、サービスの通信を許可する
```
# firewall-cmd --add-service=dns
# firewall-cmd --add-service=smtp
# firewall-cmd --add-service=http
```
また、sshサービスはより安全な設定にするために、接続元を指定して許可する
```
# firewall-cmd --add-rich-rule'rule     family=ipv4     service name=ssh    source address=10.0.2.15    accept'
```

#### サービス設定の削除
cockpid, dhcpv6-clientといった今回使用しないサービスは削除する
```
# firewall-cmd --remove-service=cockpid
# firewall-cmd --remove-service=dhcpv6-client
```


### 設定の保存
firewall-cmdで行った設定はホスト再起動後に設定が失われるので、設定の保存を行う（永続設定にする）
```
# firewall-cmd --runtime-to-permanent
```


