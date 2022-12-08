## Dockerを使用した環境構築（Rails(API)・React・MySQL@5.7）
こちらのリポジトリをクローンして作業する場合は、以下の手順を踏んでください。
```
git clone git@github.com:ooooose/Docker_setup.git
```
クローン後は、`/Docker_setup`に移動してください。
```
cd Docker_setup
```
その後、以下の手順に倣い環境構築してみてください。
なお、**基本的に全てのコマンドは`Docker_setup/`下でお願いいたします。**

### フロントエンド側の環境構築
ローカルでReactアプリを立ち上げます。<br/>
下のコマンドを実行してください。<br/>
こちらはnode.jsのバージョンは`v19.0.0`でお願いします。<br/>
併せてyarnも使用するので、事前にインストールをお願いいたします。
```
yarn create react-app front
```
その後、以下のコマンドを実行しDockerfileをfront/に移行します。
```
mv ./Dockerfile front/
```
### バックエンド側の環境構築
以下のコマンドを`/Docker_setup`で実行して、railsアプリを立ち上げます。
```
docker-compose run --no-deps api rails new . --force  -d mysql --api
```
`docker-compose`でDBに接続するために、apiディレクトリ内にある`api/config/database.yml`の上段を編集します。
```
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
作業が終わったら以下のコマンドで**必ず停止をして下さい。**
```
docker-compose down
```
再度立ち上げる場合は以下のコマンドでお願いします。
```
docker-compose up -d
```
以上になります。多少警告文は出ますが、立ち上がると思われます。<br/>
ここまでお付き合いいただき、ありがとうございました！