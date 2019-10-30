# Adding User Authentication with Devise

## This will allow users to sign in and out of our application


### Install the Devise gem

```ruby
gem 'devise'
```

```
$ bundle install
```

```
$ rails generate devise:install
```

In the terminal this generates the following output

```
===============================================================================

Some setup you must do manually if you haven't yet:

  1. Ensure you have defined default URL options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

===============================================================================

```

### How to complete the steps listed above:

```
1. Ensure you have defined default URL options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

```

## Step 1 - Setting the Default URL Options

Open `config/environments/development.rb` and add the following config.action
```ruby
Rails.application.configure do
  config.action_mailer.default_url_options = { host: 'localhost:3030' }

  # a bunch of other stuff .....
end
```

Adjust the production mailer host found in `config/environments/production.rb` and add heroku url

```ruby
Rails.application.configure do
  config.action_mailer.default_url_options = { host: 'my-great-app.herokuapp.com' }

  # a bunch of other stuff .....
end
```

If you've forgotten your heroku URL add 

In the terminal enter

```
$ heroku apps:info
```
```
Web URL: http://my-great-app.herokuapp.com
```

## Step 2 - Setting the Root URL

```
 2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"
```

This is saying we need the root of our application to go somewhere. We did this in `How to Build a Static Landing Page`.

## Step 3 - Flash Messages

```
3. Ensure you have flash messages in app/views/layouts/application.html.erb.
   For example:

     <p class="notice"><%= notice %></p>
     <p class="alert"><%= alert %></p>
```

This is telling us we should ensure the user can see the flash and alert. It gives us a bit of code that we can simply paste into `app/views/layouts/application.html.erb`

Change that code to look like this: 

```erb
</nav>

<% if notice.present? %>
  <p class="alert alert-info"><%= notice %></p>
<% end %>
<% if alert.present? %>
  <p class="alert alert-danger"><%= alert %></p>
<% end %>

<%= yield %>
```

This code adds basic alert messages, bootstrap styling, and will only display a notice or alert when one exists.

## Step 4 - Generating Devise Views

```
5. You can copy Devise views (for customization) to your app by running:

     rails g devise:views
```
This is telling you to run the following command:
```
$ rails g devise:views
```

This generates a bunch of code written for us in `app/views`

## Continue Devise Docs

Generate the user model in the terminal
```
$ rails generate devise User
```

 > NOTE: This created a migration to create a user table in our database
```db/migrate/XXXXX_devise_create_users.rb``` 

Since Devise created this migration file for us, we don't need to edit anything and can run the migration.
```
$ rake db:migrate
```

This created and added to our `schema.rb` file.
> NOTE: When you see the following in the terminal 
> ``` 
> route devise_for :users 
>```
> it implies it did something with our `config/routes.rb` file. Which in this case is it added the line
> ```
> devise_for :users
> ```
> Run rake routes to see what's been added
> ```
> new_user_session      GET  /users/sign_in(.:format)      devise/sessions#new
> new_user_registration GET  /users/sign_up(.:format)      devise/registrations#new
> new_user_password     GET  /users/password/new(.:format) devise/passwords#new
> ```
> notice `http://localhost:3030/users/sign_up` route has been created, allowing a new user to sign up.
>
> Similarly, the other URLs will allow users to sign in and reset their password.

If we keep looking in the Devise documentation under the getting started section we can see the section for Controllers, filters and helpers. This is telling us two interesting ways that we'll be able to use Devise. It says we can both add a `before_filter :authenticate_user!` to require that a user is authenticated before accessing a page, or we can access the current signed-in user using `current_user`. It's also telling us we can check to see if a user is signed in by running code that looks like `user_signed_in?`.

Open the application and adjust the navbar

```erb
<nav class="navbar navbar-toggleable-md navbar-inverse bg-inverse">
  <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo02" aria-controls="navbarTogglerDemo02" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <%= link_to 'AppName', root_path, class: 'navbar-brand' %>

  <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
    <ul class="navbar-nav mr-auto mt-2 mt-md-0">
      <li class="nav-item">
        <%= link_to new_place_path, class: 'nav-link' do %>
          <i class="fa fa-plus-circle"></i>
          New Place
        <% end %>
      </li>
    </ul>
    <ul class="navbar-nav">
      <% if user_signed_in? %>
        <li class="nav-item">
          <%= link_to 'A user is signed in!', '#', class: 'nav-link' %>
        </li>
      <% else %>
        <li class="nav-item">
          <%= link_to 'No user is signed in :(', '#', class: 'nav-link' %>
        </li>
      <% end %>
    </ul>
  </div>
</nav>
```

Go back to the terminal and run this command 
``` 
$ rake routes
```

In our routes we should see:
```
destroy_user_session DELETE /users/sign_out(.:format)      devise/sessions#destroy
```
This allows us to sign out. When we build the link we need to use the `destroy_user_session_path` URL helper and specify that we need to treat this as a `DELETE` action. 

```erb
<ul class="navbar-nav">
  <% if user_signed_in? %>
    <li class="nav-item">
      <%= link_to 'Sign out', destroy_user_session_path, method: :delete, class: 'nav-link' %>
    </li>
  <% else %>
    <li class="nav-item">
      <%= link_to 'No user is signed in :(', '#', class: 'nav-link' %>
    </li>
  <% end %>
</ul>
```

Go back to the terminal and run this command 
``` 
$ rake routes
```

In our routes we should see:
```
new_user_session GET    /users/sign_in(.:format)       devise/sessions#new
```

Open `app/views/layouts/application.html.erb` and change

```erb
<ul class="navbar-nav">
  <% if user_signed_in? %>
    <li class="nav-item">
      <%= link_to 'Sign out', destroy_user_session_path, method: :delete, class: 'nav-link' %>
    </li>
  <% else %>
    <li class="nav-item">
      <%= link_to 'Sign in', new_user_session_path, class: 'nav-link' %>
    </li>
  <% end %>
</ul>
```

Next, let's make it so if a user isn't signed in we will display a link to "Sign Up", in addition to "Sign In". If we look in our rake routes the line that looks interesting for this use case is this one:

```
new_user_registration GET  /users/sign_up(.:format)  devise/registrations#new
```

```erb
<ul class="navbar-nav">
  <% if user_signed_in? %>
    <li class="nav-item">
      <%= link_to 'Sign out', destroy_user_session_path, method: :delete, class: 'nav-link' %>
    </li>
  <% else %>
    <li class="nav-item">
      <%= link_to 'Sign in', new_user_session_path, class: 'nav-link' %>
    </li>
    <li class="nav-item">
      <%= link_to 'Sign up', new_user_registration_path, class: 'nav-link' %>
    </li>
  <% end %>
</ul>
```

## Run Git Commit Workflow
