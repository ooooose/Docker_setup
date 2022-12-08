## Dockerを使用した環境構築（Rails(API)・React・MySQL@5.7）
こちらのリポジトリをローカルにクローン後、以下の手順で環境構築してください。
### フロントエンド側の環境構築
ローカルでReactアプリを立ち上げます。<br/>
こちらはnode.jsのバージョンは19.0.0でお願いします。
```
yarn create react-app front
```
その後、ルートディレクトリで以下のコマンドを実行しDockerfileをfront/に移行します。
```
mv ./Dockerfile front/
```
### バックエンド側の環境構築
以下のコマンドをルートディレクトリで実行して、railsアプリを立ち上げます。
```
docker-compose run --no-deps api rails new . --force  -d mysql --api
```
`docker-compose`でDBに接続するために、apiディレクトリ内にある`database.yml`の上段を編集します。
```:api/config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password #ここを編集（docker-compose.ymlの指定に合わせます）
  host: db #ここも編集
```

### 全体を動かしてみる。
以下のコマンドを実行して立ち上げてみます。<br/>
```
docker-compose up -d --build
```
初回では、Rails側のDBが作成されていないので、以下のコマンドでDBを作成します。
```
docker-compose exec api rails db:create
```
### それぞれの画面
以下のポートにアクセスしてみると以下の画面になるかと思われます。<br/>
3000番port
[![Image from Gyazo](https://i.gyazo.com/fb57d8d6ad393fdf0be0ba0911f721f4.png)](https://gyazo.com/fb57d8d6ad393fdf0be0ba0911f721f4)

8000番port
[![Image from Gyazo](https://i.gyazo.com/c14552f1923434522dedc17bca9032ed.png)](https://gyazo.com/c14552f1923434522dedc17bca9032ed)

### 終了と再度立ち上げる場合
作業が終わったら以下のコマンドで停止をして下さい。
```
docker-compose down
```
再度立ち上げる場合は以下のコマンドでお願いします。
```
docker-compose up -d
```
以上になります。ありがとうございました！