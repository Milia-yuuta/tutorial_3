#環境構築手順
##各種コンテナのBuild
$ docker-compose build
##ライブラリの準備(railsコマンドが有効になる)
$ docker-compose run --rm app bundle install
##Railsの初期化
$ docker-compose run --rm app bundle exec rails new . -f -B -d postgresql
##Railsの依存ライブラリがGemfileに書き出されるのでもう一回bundle install
$ docker-compose run --rm app bundle install
##webpackerの追加(Pumaを立ち上げようとすると怒られるため追加)
$ docker-compose run --rm app bundle exec rails webpacker:install
##database.ymlの編集(DB設定をしないとDB接続エラーになる)
config/database.yml
```
default: &default
adapter: postgresql
encoding: unicode
pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
<<: *default
host: <%= ENV['DATABASE_HOST'] %>
database: <%= ENV['DATABASE_NAME'] %>
username: <%= ENV['DATABASE_USER'] %>
password: <%= ENV['DATABASE_PASSWORD'] %>

test:
<<: *default
host: <%= ENV['DATABASE_HOST'] %>
database: app_test
username: <%= ENV['DATABASE_USER'] %>
password: <%= ENV['DATABASE_PASSWORD'] %>

production:
<<: *default
database: app_production
username: app
password: <%= ENV['APP_DATABASE_PASSWORD'] %>
```

##起動
$ docker-compose up

ブラウザでhttp://localhost
例の画面が表示される