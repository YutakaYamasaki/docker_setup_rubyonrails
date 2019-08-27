# docker_setup_rubyonrails
Rails+MySQLのDockerファイル

## 使い方

### このファイルをgit cloneする

```
$ git clone git@github.com:YutakaYamasaki/docker_setup_rubyonrails.git
```

### アプリ名を変更する。

クローンしてきたフォルダの名前と`docker-compose.yml`と`entrypoint.sh`に記載されている`app_name`から独自のアプリ名に変えてください。
(そのまま使用しても構いません。)

### Railsアプリケーションの作成

```
$ docker-compose run web rails new . -d mysql --skip-bundle
```

### `database.yml`の変更
下記に変更してください。
独自のアプリ名に変更している場合は`app_name`の部分を変更してください。
```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: <%= ENV['DB_HOST'] %>
development:
  <<: *default
  database: app_name_development
test:
  <<: *default
  database: app_name_test

# 本番環境でHerokuを使用する場合は下記をコメントアウトしてください。

# production:
#   <<: *default
#   adapter: postgresql
#   encoding: unicode
#   pool: 5
```

### Buildする。

```
$ docker-compose build
```

### サーバーを立ち上げる

```
$ docker-compose up -d
```

### 立ち上がったアプリケーションを確認したい時

下記のURLに飛ぶ
http://localhost:3002/

### サーバを落とす

```
$ docker-compose down
```

### railsのコマンドを使用したい場合

```
# サーバーを立ち上げている場合
$ docker-compose exec web railsコマンド
# サーバーを落としている場合
$ docker-compose run web railsコマンド
```
