# サーバー側で設定されるべきTLSの暗号方式の設定

## 検証した設定など

- final

SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA:!MD5

- draft

SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA:!SHA1:!MD5
SSLHonorCipherOrder on

- draft2

SSLProtocol all -SSLv3
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA
SSLHonorCipherOrder on


## 考察

SSL2.0/3.0は、ダウングレード攻撃、バージョンロールバック攻撃、BEAST、POODLEなど脆弱な問題があるため無効。

TLS1.0/1.1は、使用するCipherSuiteにもよるが、脆弱性が存在する。相互接続性維持のためにのみ使用されるべき。

以上より、プロトコルに関しては極力TLS1.2、1.3を使用したい。(2021年時点)

また、CipherSuiteに関しては上記の例だと、SEEDやARIAなど一部問題のあるCipherSuiteが有効になる可能性がある。利用する環境により適宜調整が必要だと思われる。

なお、CipherSuiteの選定にあたって、よりセキュアにホワイトリスト方式を採用する方法もあるが、クライアントの多様性により選定が難しいと考える。脆弱な問題が見つかっている暗号化方式(3DES,RC4など)を最低限無効化しておけば良いと考え、ブラックリスト方式を採用した。

上記設定はApacheに関する例。Apacheには他にもTLS接続に関する設定項目があるが、[SSLHonorCipherOrder](https://httpd.apache.org/docs/current/mod/mod_ssl.html#sslhonorcipherorder)は無効とした。TLS接続に使用する暗号化方式の優先順序をサーバー側の設定を優先するために使用される。一部サイトには「ダウングレード攻撃を防ぐ」と説明がある。しかし、影響のあるSSL2.0/3.0を無効にしている点とサーバー側の設定を優先することで一部クライアントが接続できなくなる可能性も考慮してあえて無効にした。

