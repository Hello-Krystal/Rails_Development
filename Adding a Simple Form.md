#How to Add a Simple Form

## Installing and Setting up Simple Form

Place in `gemfile`
``` ruby
gem 'simple_form'
```

```
$ bundle install
```

Run the simple form install command in terminal

```
$ rails generate simple_form:install --bootstrap
```

This process generates a file in `config/initializers`. Whenever changes happen in this folder, we need to restart our server.