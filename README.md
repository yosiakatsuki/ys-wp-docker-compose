# Docker for Macで作るWordPress開発環境

## .localドメインでアクセスする方法

別で`nginx-proxy`イメージを使ってドメインごとに振り分けできるようにしておく
（`/share/docker-compose.yml`参照）

**Browsersyncを使う場合はドメインを`.local`にして`proxy`指定すると遅くなるので注意**

下記ネットワークドライバーを作成・指定する必要がある。

```
networks:
  default:
    external:
      name: nginxproxy_default
```

ネットワークを追加する場合は`docker network create`する。

```
$ docker network create [名前]
```

### hostsの書き換え

ドメインでアクセスできるように`hosts`の書き換えも必要。

```
$ sudo vi /etc/hosts
```

### SSL対応

自己証明書（オレオレ証明書）をローカルで作る

複数ホストに対応させるために「Subject Alternative Name」を使用する

調査・実験中…

`./data/certs`に保存する。（コンテナの`/etc/nginx/certs`と同期させているフォルダに保存。）

## WordPressのメディアアップロード上限が2MBになる対策

`php.ini`の初期値が2MBなのでiniを追加して上限を上げる。
(`/wordpress/php/ini/uploads.ini`参照)

`volumes`で`./php/ini/uploads.ini:/usr/local/etc/php/conf.d/uploads.ini`を同期させておく。

### ついでにnginxも

`/share/conf/server.conf`でPHPのアップロード上限と合わせる。
`volumes`で`./conf/server.conf:/etc/nginx/conf.d/server.conf`を同期。

## dockerコンテナに入る

```
$ docker exec -it コンテナ名 bash
```