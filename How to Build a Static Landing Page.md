# Build the Landing Page

```
$ rails generate controller static_pages
```

This command generates the `StaticPagesController` <br/>
To connect the landing page to the `index` action inside `app/controllers/static_pages_controller.rb`

```ruby 
class StaticPagesController < ApplicationController

  def index
  end

end
```

Build a view for the index action that has dummy content by creating an `index.html.erb` file in `app/views/static_pages`

Hook up `config/routes.rb` so `localhost:3030` can take us to the landing page

```ruby
Rails.application.routes.draw do
  root 'static_pages#index'
end
```