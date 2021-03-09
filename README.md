# ssl-tls_verification

SSL/TLSの検証環境

## 環境構成

- CentOS 7.9
- Apahce/2.4.6-93.el7.centos
- OpenSSL/1.0.2k-19.el7

## 使用方法

1. `centos7/ssl/`にサーバの秘密鍵を`server.key`として、サーバの証明書を`server.crt`として作成する。

2. docker-compose build

3. docker-compose up -d

