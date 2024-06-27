### ネットワーク設定
|No |項目 |設定/<br>アプリケーション名 |内容 |動作可否 |
|---|---|---|---|---|
|01 |IPアドレス |ネットワーク設定 |ipコマンドによるIPアドレス確認 | |
|02 |ホスト名 |ネットワーク設定 |hostnameコマンドによるホスト名確認 | |
|03 |DNSサーバ・<br>検索ドメイン |ネットワーク設定 |catコマンドによるDNSサーバと検索ドメインの確認（/etc/resolv.conf） | |
|04 |通信確認 |ネットワーク設定 |pingコマンドでlocalhost, IPアドレス, ネット上のサイトにパケットを送信して通信確認 | |


---
### firewall
|No |項目 |設定/<br>アプリケーション名 |内容 |動作可否 |
|---|---|---|---|---|
|01 |ルール |パケットフィルタリングのルール設定 |firewall-cmd --list-allコマンドでルールが正しく設定されているか確認 | |
|02 |名前解決の実行 |サービス設定 |digコマンドで名前解決の確認 | |
|03 |Webサーバへのアクセス |サービス設定 |ブラウザにIPアドレスを入力して、Webページを確認 | |
|04 |自ホストのstudentユーザ宛のメール送信 |サービス設定 |（NATネットワーク内の）別の仮想マシンからstudentユーザ宛にメールを送信 | |
|05 |自ホストへSSH接続 |サービス設定 |（NATネットワーク内の）別の仮想マシンからSSH接続をする | |

### メールサーバー構築

|　NO　|　項目　|　設定、アプリケーション名　|　内容　|　動作　|
|---|---|---|---|---|
|　1　|　設定変更　|　main.cf(postfix)　|　設定項目確認(構築手順書内の表参照)　|　　　|
|　2　|　設定変更　|　/etc/aliases　|　root:studentに変更　|　　　|
|　3　|　設定変更　|　/etc/dovecot/dovecot.conf　|　プロトコルをPOP3に変更　|　　　|
|　4　|　コンパイル準備、起動　|　dovecot,postfix　|　studentでファイルが開けるか確認　|　　　|
|　5　|　設定変更　|　メールボックスの設定　|　受信サーバーの変更、ユーザー名入力など　|　　　|
|　6　|　動作確認　|　メールの送受信確認　|　sendmail / mailq　|　　|　　|
|　7　|　動作確認　|　/etc/aliases　|　student宛てのメールが転送されているか確認　|　　|　　|

### Wedサーバー構築

|No |項目 |設定/<br>アプリケーション名 |内容 |動作可否 |
|---|---|---|---|---|
|01 |Webサーバ |Apache HTTP Server |http://www.r-excercise.com | |
|02 |Webサーバ |PHP |PHPでファイルの作成削除ができるか　| |
|03 |Webサーバ |Apache HTTP Server |https://IPアドレス | |

### ネームサーバー, SSH

 <!-- name -->
<table>
  <caption>チェックシート</caption>
  <thead>
    <tr>
      <th>No</th> <th>項目</th><th>設定/アプリケーション名</th><th>内容</th><th>動作可否</th>
    </tr>
  </thead>
  <tr>
    <td> 01</td> <td>ネームサーバ</td><td>BIND</td><td># ss -ntulp | grep namedでTCP53とUDP53がリッスンされている</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 02</td> <td>ネームサーバ</td><td>BIND</td><td># dig r-exercise.com. NSでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 03</td> <td>ネームサーバ</td><td>BIND</td><td># dig r-exercise.com. MXでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 04</td> <td>ネームサーバ</td><td>BIND</td><td># dig ns.r-exercise.com. AでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 05</td> <td>ネームサーバ</td><td>BIND</td><td># dig mail.r-exercise.com. AでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 06</td> <td>ネームサーバ</td><td>BIND</td><td># dig h015.r-exercise.com. AでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>
  <tr>
    <td> 07</td> <td>ネームサーバ</td><td>BIND</td><td># dig www.r-exercise.com. CNAMEでANSWER SECTIONが返ってくる</td><td>〇か×</td>
  </tr>

  <!-- ssh -->
  <tr>
    <td> 01</td> <td>ssh</td><td>OpenSSH</td><td>/etc/ssh/sshd_configでPasswordAuthenticationが適切な設定になっている</td><td>〇か×</td>
  </tr>
</table>



