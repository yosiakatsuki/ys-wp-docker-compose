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

自己証明書（オレオレ証明書）をローカルで作る

複数ホストに対応させるために「Subject Alternative Name」を使用する

調査・実験中…

`./data/certs`に保存する。（コンテナの`/etc/nginx/certs`と同期させているフォルダに保存。）

## dockerコンテナに入る

```
$ docker exec -it コンテナ名 bash
```