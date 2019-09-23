# Docker

## 
docker-compose up -d

コンテナを作成して実行するよ
-dをつけないと、メッセージとか出続けるよ（みたい場合はそれもOK）

docker-compose stop

コンテナの実行を停止するよ
これならdbは消えないよ

docker-compose start

コンテナの実行するよ（再開）

docker-compose down

コンテナを停止して削除するよ
コンテナ削除だからdbも消えるよ

docker-compose down --rmi all

加えてイメージも削除するよ

# down前にdBバックアップ

docker exec -it docker-slide_db_1 sh -c 'mysqldump --all-databases -u root -ppassword 2> /dev/null > /var/lib/mysql/mysql.dump.sql'

# ref.
Dockerイメージの理解とコンテナのライフサイクル https://www.slideshare.net/zembutsu/docker-images-containers-and-lifecycle

# gitPitch

https://gitpitch.com/kou10mo/docker-slide

# ドメインでアクセスしたい場合(proxy)

[Dockerコンテナへのアクセスをドメインごとに変更する「nginx\-proxy」レビュー \| さくらのナレッジ](https://knowledge.sakura.ad.jp/3142/)

```
# proxy用のネットワーク作成
docker network create proxy

# proxy用ネットワークを指定して実行(デフォルトのbridgeはcomposeで使用できないため)
docker run --net=proxy -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy

# docker-genのインストール(必要…？)
# 上記さくらのナレッジ参照
mkdir -p /tmp/templates && cd /tmp/templates
curl -o nginx.tmpl https://raw.githubusercontent.com/jwilder/docker-gen/master/templates/nginx.tmpl
docker run -d --name nginx-gen --volumes-from nginx \
 -v /var/run/docker.sock:/tmp/docker.sock \
 -v /tmp/templates:/etc/docker-gen/templates \
 -t jwilder/docker-gen:0.3.4 -notify-sighup nginx -watch \
 --only-published /etc/docker-gen/templates/nginx.tmpl /etc/nginx/conf.d/default.conf

# 環境変数(VIRTUAL_HOST: you-want.localhostなど)を指定してコンテナ実行すればOK
```
