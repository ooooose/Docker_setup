# Dockerを使用した環境構築（Rails(API)・React・MySQL@5.7）
こちらのリポジトリをローカルにクローン後、以下の手順で環境構築してください。
## バックエンド側の環境構築
### Railsアプリケーションを作成
以下のコマンドを実行して、railsアプリを立ち上げます。
```
$ docker-compose run --no-deps api rails new . --force  -d mysql --api
```
`docker-compose`でDBに接続するために、apiディレクトリ内にある`database.yml`の上段を編集します。
```:api/config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password #ここを編集
  socket: /tmp/mysql.sock
  host: db #ここを追加
```
## フロントエンド側の環境構築
以下のコマンドを実行して、Reactを立ち上げます。
```
docker-compose run --rm front sh -c "yarn create react-app app"
```
## 全体を動かしてみる。
以下のコマンドを実行して立ち上げてみます。
```
docker-compose up -d
```
初回では、Rails側のDBが作成されていないので、以下のコマンドでDBを作成します。
```
docker-compose exec api rails db:create
```