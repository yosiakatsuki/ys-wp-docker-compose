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

SSLについてはまだ未検証（2018/12/03）


ドメインでアクセスできるように`hosts`の書き換えも必要

```
$ sudo vi /etc/hosts
```