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
