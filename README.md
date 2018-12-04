# Docker for Macで作るWordPress開発環境

## .localドメインでアクセスする方法

別で`nginx-proxy`イメージを使ってドメインごとに振り分けできるようにしておく
（`nginx-proxy-docker-compose.yml`参照）

下記ネットワークドライバーを作成・指定する必要がある。

```
networks:
  default:
    external:
      name: aktk_local
```

ドメインでアクセスできるように`hosts`の書き換えも必要。

```
$ sudo vi /etc/hosts
```

### SSL対応

オレオレ証明書をローカルで作る

`xxxx.local`のドメインをまとめてSSL対応できるように`local.key`のような名前で作る。

```
$ openssl genrsa 2048 > local.key
$ openssl req -new -key local.key > local.csr
# いろいろ訊かれるので適当に入力
# パスワードだけは空にする
# ----
# 有効期限も適当に長くしておく
$ openssl x509 -in local.csr -days 10000 -req -signkey local.key > local.crt
```

`./data/certs`に保存する。（コンテナの`/etc/nginx/certs`と同期させているフォルダに保存。）

## dockerコンテナに入る

```
$ docker exec -it コンテナ名 bash
```