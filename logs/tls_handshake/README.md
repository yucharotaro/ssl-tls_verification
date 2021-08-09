# 古いプロトコルバージョンとのハンドシェイクに関して

## 前提

- サーバー側で有効なプロトコルバージョンがSSLv2.0/3.0,TLSv1.0。

- クライアント側で有効なプロトコルバージョンはTLSv1.0/1.1/1.2/1.3。

## 事象

OpenSSLでオプションなしで接続を試行するも、接続が拒否された。(FINパケットが返る。)しかし、オプションにCipherを指定するとTLS1.0で接続された。

## 調査

参考:

- [RFC5246 The Transport Layer Security (TLS) Protocol Version 1.2](https://datatracker.ietf.org/doc/html/rfc5246)

- [RFC8446 The Transport Layer Security (TLS) Protocol Version 1.3](https://datatracker.ietf.org/doc/html/rfc8446)


RFC5246によると、TLSハンドシェイク時、クライアントが送信するClientHelloは、クライアントがサポートする最新のプロトコルバージョンをProtocolVersion client_versionに指定することになっている。

RFC5246には、下位互換性に関して以下の引用部分の説明があった。

> Note: some server implementations are known to implement version negotiation incorrectly. For example, there are buggy TLS 1.0 servers that simply close the connection when the client offers a version newer than TLS 1.0. Also, it is known that some servers will refuse the connection if any TLS extensions are included in ClientHello. Interoperability with such buggy servers is a complex topic beyond the scope of this document, and may require multiple connection attempts by the client.

一部の実装では、バージョンネゴシエーションが正しく実装されていないため、クライアントが新しいバージョンのプロトコルをネゴシエーションすると接続を閉じるだけのバグが存在するという。また、TLSの拡張機能がClientHelloに含まれる場合、一部のサーバーは接続を拒否するという。そういった場合は、クライアントによる複数の接続試行をする必要があるとのこと。

今回の事象に関して、何故かCipherSuiteを指定することで接続できたが、TLSのサーバー側の実装によって接続を拒否されるバグを踏んだ可能性が高いと思われる。

ちなみに、RFC8446によると、TLS1.3のClientHelloではExtenion supported_versionが使用できる。これはクライアントがサポートするTLSのバージョンを複数示すために、バージョンのリストを優先度を決めてサーバーに送信することができる。この拡張がClientHelloに存在する場合は、ネゴシエーションにClientHelloのProtocolVersion client_versionを使用してはならないとある。

