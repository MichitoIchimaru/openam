# openam
OpenAMの公式Dockerイメージである openidentityplatform/openam をSSL化し、かつ設定内容を永続化させるdocker-compose  
※本設定は検証目的であり、この内容で実際のIdP構築を推奨するものではありません。

## 準備
### サーバー証明書作成
下記コマンドでサーバ証明書を作成する。コマンドは本リポジトリ直下（docker-compose.yamlファイルと同じ場所）で実行することを想定。

``` sh
openssl req -x509 -sha256 -nodes -days 3650 -newkey rsa:2048 -subj /CN=idp.example.com -keyout openam/cert/server.key -out openam/cert/server.crt
```
### OpenAM configフォルダ権限設定
Linux上でdocker起動する場合、下記コマンドでコンテナ側からバインドマウントしたフォルダにファイルを書き込めるようにする。コマンドは本リポジトリ直下（docker-compose.yamlファイルと同じ場所）で実行することを想定。  
コンテナ上ではopenamユーザーがID:1001で作成されているため、フォルダの所有者を1001とする。ホスト上1001がopenamである必要はなく、IDが一致していればよい。また、1001ユーザーが存在していなくてもよい。

``` sh
chmod 755 openam/config
chown 1001 openam/config
```

### hostsファイル更新
hostsファイルにidp.example.comを追加し、ホストOSのIPアドレスを定義する。

## 起動
下記コマンドでコンテナを起動する。

``` sh
docker-compose up
```

https://idp.example.com:8443/openam にアクセスし、初期設定を実施すると openam/config フォルダにOpenAMの諸情報が書き込まれる。