# Adding Bootstrap

Add files to our `gemfile`

If we're missing ``gem 'jquery-rails'`` install jQuery

## How to install jQuery
```
gem 'jquery-rails'
```
```
bundle install
```
Open `assets/javascripts/application.js` and add the following above turbolinks

```javascript
//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require_tree . 
```

*** Restart Rails Server ***

Add the following lines to the bottom of the gemfile
```
gem 'popper_js', '~> 1.11.1'
gem 'bootstrap', '4.0.0.alpha6'

source 'https://rails-assets.org' do
  gem 'rails-assets-tether', '>= 1.3.3'
end
```

run a bundle install and restart the server
``` 
$ bundle install 
```

The bootstrap gem is looking for application.scss but we only have application.css to change that. 

Use the move command (mv) to rename the file. 

```
$ mv app/assets/stylesheets/application.css app/assets/stylesheets/application.scss
```

create a `master.scss` stylesheet to import bootstrap

```ruby
@import "bootstrap";
```

Finally, adjust `app/assets/javascripts/application.js` to include: 

``` javascript
//= require turbolinks
//= require popper
//= require tether
//= require bootstrap-sprockets
//= require_tree .
```