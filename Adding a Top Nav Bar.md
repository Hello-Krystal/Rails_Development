# How to add a Top Navigation Bar

Open `app/views/layouts/application.html.erb` and insert the following into the `body`

```html
<!DOCTYPE html>
<html>
<head>
    <title>
    </title>
</head>

<body>

<nav class="navbar navbar-toggleable-md navbar-inverse bg-inverse">
  <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo02" aria-controls="navbarTogglerDemo02" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <a class="navbar-brand" href="#">APP NAME</a>

  <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
    <ul class="navbar-nav mr-auto mt-2 mt-md-0">
      <li class="nav-item">
        <a class="nav-link" href="#">LINK TO PAGE</a>
      </li>
    </ul>
  </div>
</nav>

    <%= yield %>

</body>
</html>
```