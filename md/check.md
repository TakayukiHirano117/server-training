<table>
  <caption>チェックシート</caption>
  <!-- ネームサーバ -->
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


