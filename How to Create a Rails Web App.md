# Create a new application that uses Postgres

```
$ rails new appname --database=postgresql 
```

### Adjust database.yml

```ruby efault: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: postgres
  password: password
  host: localhost

development:
  <<: *default
  database: appname_development

test:
  <<: *default
  database: appname_test

production:
  <<: *default
  database: appname_production
  # username: appname
  # password: <%= ENV['APPNAME_DATABASE_PASSWORD'] %>
  ```

### Create intial and empty database

```
$ rake db:create
```