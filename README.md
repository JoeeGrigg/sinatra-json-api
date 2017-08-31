![Sinatra JSON API Logo](https://cloud.githubusercontent.com/assets/8267620/14919215/b04b1818-0e1f-11e6-9e8a-9b5aeadd2d4c.jpg)

# Sinatra JSON API

A [Sinatra](http://www.sinatrarb.com/) JSON API starter application.

This is my take on a sinatra json api. It includes a range of nice to have helpers
that help speed up development and try to keep your controllers as small as
possible.

The app is set up to support sqlite databases throughout all stages of development
but once in production will expect you to use [PostgreSQL](https://www.postgresql.org/).
However, this can easily be changed and will support any database that
[ActiveRecord](https://github.com/rails/rails/tree/master/activerecord) supports.

## Table of Contents

* [Tools Used](#tools-used)
* [Getting Started](#getting-started)
* [Project Structure](#project-structure)
* [Docs](#docs)
  * [Making Requests](#making-requests)
  * [Development Environment Variables](#development-environment-variables)
  * [Controllers](#controllers)
  * [Helpers](#helpers)
  * [Migrations](#migrations)
  * [Starting the Server](#starting-the-server)

## Tools Used

* **Sinatra** - a Ruby MVC framework.
* **JSON** - json data handling.
* **ActiveRecord** - database mapping.
* **Shotgun & Tux** for development, automatic app reloads and repl.

## Getting started

This is the getting started guide which will get you in a state where you can start
developing your application.

* Clone the project onto your machine.

* Run `bundle install` to install Ruby gems.

  This will download and install of the your dependancies.

* Run `rake db:migrate` to setup the development database.

  This will setup the database you you.

* Run `bundle exec shotgun --server=thin --port=9292 config.ru` to start local server.

  This will start your applications server using shotgun so that any time you make a
  change it is automatically loaded in.

## Project Structure

* app/
	* helpers/ - small utility classes & functions.
	* models/ - `ActiveRecord::Base` subclasses.
	* controllers/ - the controllers/routes for your app.
* config/ - config files, such as the database configuration.
* db/ - the database schema and migrations.
* setup/ - setup files which load in libraries and base configuration.
* Procfile - server startup config for services suck as Heroku.
* app.rb - starting point of the app.
* config.ru - Rack server startup config.

## Docs

### Making Requests

As this is a JSON API. It will only accept JSON as content and will only ever return
JSON in its response.

#### Example

```bash
curl -X GET http://localhost:9292
```

### Development Environment Variables

You can put all of your environment variables during development inside of
`config/environment.yml`. As long as this file exists it will be loaded in when the
server starts.

### Controllers

Adding a controller is as simple as creating a new file inside the `app/controllers`
folder or child directories of that and then going forward as if you where
writing any other sinatra endpoint.

#### Example

```ruby
# 'app/controllers/my_new_controller.rb'
get "/mynewendpoint"

  # Endpoint code goes here

end
```

### Helpers

Helpers within this project are a mixture of standard Sinatra helpers and custom
classes. Currently all sinatra helpers are located in 'app/helpers/sinatra' and
custom classes are just in 'app/helpers'. However, this folder structure doesn't
need to be followed. Everything inside 'app/helpers' will be loaded into the app.

### Included Sinatra Helpers

This project includes two standard sinatra helpers, "get_objects" and "output".

These two helpers will both work on their own however add the most functionality
when used together.

When they are used together they add three key bits of functionality to your
endpoint without anything else being added by yourself, optional paging, sorting and filtering.

#### Examples

##### Paging

```
http://localhost:9292/items?page=1
```

##### Sorting:

A sorting field and order must be specified.

Order can either be ```ASC``` or ```DESC```

```
http://localhost:9292/items?sort=field:ASC
```

##### Filtering

Filtering can either be done by specifying fields that match a specific term or
do not.

When specifying a field that should not match a specific term start its name with a ```!```

```
Matching a field:
http://localhost:9292?field_name=term

Not matching a field:
http://localhost:9292?!field_name=term
```

You can specify as many matching and not matching fields as you like in one query.

These can also be combined.

#### Making helpers available elsewhere within your app

Sinatra helpers will only be accessible from within other Sinatra helpers and controllers.

If you wish to access your Sinatra helpers in a model or anywhere that is not a
Sinatra helper or a controller then you must use a custom class style helper.

### Migrations

* Run `rake db:create_migration NAME=####` and replace #### with the name of the migration.
* Make changes to the migration found in `db/migrate`. These are standard ActiveRecord migrations.
* Run `rake db:migrate` to update the database.

### Starting the Server


To start the server in development I would suggest using [Shotgun](https://github.com/rtomayko/shotgun).
While it is quite a lot slower, in development it will save you time because it
will auto load changes as you make them and create new files.

To start the server with this use: ```bundle exec shotgun --server=thin --port=9292 config.ru```

In production I would suggest [Thin](http://code.macournoyer.com/thin/) as it is
extremely fast.

To start the server with this use: ```bundle exec rackup```

---

© Joe Grigg 2017
