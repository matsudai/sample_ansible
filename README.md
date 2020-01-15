# Vagrant + Ansible (Docker, Ruby and Ruby on Rails)

## Install Docker, Ruby and Ruby on Rails using Ansible in build Vagrant instance

```bash
> vagrant up
```

## Getting Started Sample Rails Application (Rails 6.0, PG 12.1)

Configure for using postgres client in Rails.

```bash
$ export POSTGRES_HOST=localhost
$ export POSTGRES_PORT=5432
$ export POSTGRES_USER=postgres
$ export POSTGRES_PASSWORD=password
```

Copy sample Rails application. (If you donot use sample, remove synced_folder from Vagrantfile.)

```bash
$ cp -r /tmp/sample_rails ./
$ cd sample_rails
$ docker-compose up -d
```

Create new Rails project.

```bash
$ rails new web --database=postgresql
$ cd web
```

Update config for DB connection.

```bash
$ vi config/database.yml
```

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

Create DB and install webpacker.

```bash
$ rails db:create
$ rails webpacker:install
```

Scaffold sample feature.

```bash
$ rails g scaffold user user_number:string first_name:string last_name:string
$ rails db:migrate
```

Start Rails server.

```bash
$ rails s -p 3000 -b 0.0.0.0
```

=> localhost:3000/users
