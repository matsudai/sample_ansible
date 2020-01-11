# Vagrant + Ansible + Docker

## Getting Started Sample Rails Application (Rails 6.0, PG 12.1)

$ export POSTGRES_HOST=localhost
$ export POSTGRES_PORT=5432
$ export POSTGRES_USER=postgres
$ export POSTGRES_PASSWORD=password

$ cp -r /tmp/sample_rails ./
$ cd sample_rails
$ docker-compose up -d

$ rails new web --database=postgresql
$ cd web
$ vi vi config/database.yml

```yaml
 default: &default
   adapter: postgresql
   encoding: unicode
+  host: <%= ENV.fetch('POSTGRES_HOST') %>
+  port: <%= ENV.fetch('POSTGRES_PORT') %>
+  user: <%= ENV.fetch('POSTGRES_USER') %>
+  password: <%= ENV.fetch('POSTGRES_PASSWORD') %>
   # For details on connection pooling, see Rails configuration guide
   # https://guides.rubyonrails.org/configuring.html#database-pooling
   pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
```

$ rails db:create
$ rails webpacker:install

$ rails g scaffold user user_number:string first_name:string last_name:string
$ rails db:migrate

$ rails s -p 3000 -b 0.0.0.0

=> localhost:3000/users
