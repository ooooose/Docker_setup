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
ローカルでReactアプリを立ち上げます。
```
yarn create react-app front
```
その後、ルートディレクトリで以下のコマンドを実行しDockerfileをfront/に移行します。
```
mv ./Dockerfile front/
```
## 全体を動かしてみる。
以下のコマンドを実行して立ち上げてみます。<br/>
```
docker-compose up -d --build
```
初回では、Rails側のDBが作成されていないので、以下のコマンドでDBを作成します。
```
docker-compose exec api rails db:create
```
## それぞれの画面
3000番port

8000番port

## 終了と再度立ち上げる場合
作業が終わったら以下のコマンドで停止をして下さい。
```
docker-compose down
```
再度立ち上げる場合は以下のコマンドでお願いします。
```
docker-compose up -d
```