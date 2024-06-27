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





