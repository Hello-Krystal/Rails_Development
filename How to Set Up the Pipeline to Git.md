# Set Up Pipeline

## To Github:

``` 
$ git init
```

``` 
$ git add --all
```

``` 
$ git commit -am "Initial commit"
```

## To Heroku:

Creates project:
```
$ heroku create appname
```

Pushes it to github:
```
$ git push heroku master
```

To find out your Heroku URL: 
```
$ heroku apps:info
```