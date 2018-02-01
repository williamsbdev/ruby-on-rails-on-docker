# ruby-on-rails-on-docker

This is a living code example (step-by-step) inspired by
https://docs.docker.com/compose/rails

### Steps

This will repo will provide commit points so that it will be
easy to checkout each step to follow along in case mistakes
are made. This example was done on a MacBook with Docker
version 18.01. Your example may differ based on platform.

#### Step 1

    docker build -t ruby-on-rails .

    docker run -it -v "${PWD}:/app" ruby-on-rails /bin/bash

While running interactively in the Docker container, we're going to install the
`rails` gem.

    gem install rails -v 5.0.0.1

    rails new app --force --database=postgresql

#### Step 2

You should now have rails installed and a new rails
application. You may exit the container by typing `exit`.

#### Step 3

Now we can finish our Dockerfile and build our image to run
our application.

    docker build -t ruby-on-rails .

    docker run --name web -p 3000:3000 ruby-on-rails bundle exec rails s -p 3000 -b '0.0.0.0'

#### Step 4

This results in an error though because we did not configure
the PostgreSQL database connection properly. We need to add
the following to our `config/database.yml`:

    default: &default
      adapter: postgresql
      encoding: unicode
      host: db
      username: postgres
      password:
      pool: 5

Also, instead of forcing ourselves to rebuild our container
each time, we will mount our code directly into the container.

    docker run -v "${PWD}:/myapp" --name web -p 3000:3000 ruby-on-rails bundle exec rails s -p 3000 -b '0.0.0.0'

We encounter another error though because we are not running
our PostgreSQL database container.

    docker run --name db postgres

    docker run -v "${PWD}:/myapp" --name web --link db -p 3000:3000 ruby-on-rails bundle exec rails s -p 3000 -b '0.0.0.0'

#### Step 5

Now we get a database does not exist error. We can fix that by
running the `rake db:create` in our web container.

    docker exec web rake db:create

Finally, we have a fully functioning Ruby on Rails
application.
