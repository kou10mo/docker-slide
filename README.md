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
