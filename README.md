
# Prelude API

Minimal boiler plate to create a dockerized Ruby on Rails 7 API only application.

## Clone the application
```
git clone https://github.com/betogrun/prelude-api.git your_application
```
or

```
git clone git@github.com:betogrun/prelude-api.git your_application
```

## Change the application name references

Change the application name on Dockerfile

```yml
WORKDIR /usr/src/your_application
```

Change the application name on docker-compose.yml

```yml
 web:
    build: .
    entrypoint: ./entrypoint.sh
    command: bash -c "bundle exec rails s -p 3000 -b '0.0.0.0'"
    tty: true
    volumes:
      - .:/usr/src/your_application
```

## Generate the Rails project

Generate an application with Postgres database
Feel free to change the options, but keep in mind the next steps may be affected.

```
docker-compose run --rm --no-deps web bundle exec rails new . --api --force -d postgresql 
```

Change the files ownership if you are using Linux.
```
sudo chown -R $USER:$USER .
```

## Update the database configuration

Add the following content to `config/database.yml` default entry.

```yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: <%= ENV.fetch("DATABASE_HOST") %>
  username: <%= ENV.fetch("DATABASE_USER") %>
  password: <%= ENV.fetch("DATABASE_PASSWORD") %>
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
```

## Create the database
```
docker-compose run --rm web bundle exec rails db:create
```

## Running the application
```
$ docker-compose up
```

## Debugging

Get the id for the web container

```
docker ps
```

Attach your terminal to the container

```
docker attach container_id
```
