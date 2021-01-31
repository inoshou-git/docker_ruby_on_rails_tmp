# docker_ruby_on_rails_tmp(docker ruby on rails開発環境)
docker x ruby v2.6.6 x rails v6.1.1 x mysql2

## セッティング
任意の場所にプロジェクトフォルダ作成
```
mkdir myapp
```

rails newでアプリ作成
```
docker-compose run web rails new . --force --no-deps --database=mysql --skip-test --webpacker
```

イメージのビルド
```
docker-compose build
```

config/database.ymlの設定とDB接続
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("MYSQL_USERNAME", "root") %>
  password: <%= ENV.fetch("MYSQL_PASSWORD", "password") %>
  host: <%= ENV.fetch("MYSQL_HOST", "db") %>

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

データベース作成
```
docker-compose run web rake db:create
```

コンテナ起動
```
docker-compose up
```

## 参考
[【Rails】Rails 6.0 x Docker x MySQLで環境構築](https://qiita.com/nsy_13/items/9fbc929f173984c30b5d) を参考にさせていただきました。
