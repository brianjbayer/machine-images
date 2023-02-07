# Dockerfile for a containerized development environment
# for the https://learn-rails.com/content/learnrails2/
# Rails tutorial
# ASSUMPTION: source is volume mounted
# docker build --no-cache -t brianjbayer/rails-7.0.4-ruby-3.1.3-dev .
# docker run -it --rm -v $(pwd):/app -p 3000:3000 brianjbayer/rails-7.0.4-ruby-3.1.3-dev
# bundle exec bin/rails server -p 3000 -b 0.0.0.0

ARG BASE_IMAGE=ruby:3.1.3-slim-bullseye
FROM ${BASE_IMAGE}

ARG BUNDLER_VER=2.4.1
ARG RAILS_VER=7.0.4

# Install Build Packages, Bundler, and rails
ARG BUILD_PACKAGES='build-essential libpq-dev libsqlite3-dev'
RUN apt-get update \
  && apt-get -y dist-upgrade \
  && apt-get -y install ${BUILD_PACKAGES} \
  && rm -rf /var/lib/apt/lists/* \
  # install bundler and rails versions
  && gem install bundler:$BUNDLER_VER rails:$RAILS_VER

# --- Dev Environment ---
WORKDIR /app

# Add git, vim, and curl at least
ARG DEVENV_PACKAGES='git vim curl'
RUN apt-get update \
  && apt-get dist-upgrade \
  && apt-get install -y ${DEVENV_PACKAGES} \
  && rm -rf /var/lib/apt/lists/*

# Add console IRB config
RUN echo "IRB.conf[:PROMPT_MODE] = :SIMPLE" >> ~/.irbrc \
  && echo "IRB.conf[:AUTO_INDENT_MODE] = false" >> ~/.irbrc

# Start devenv in (command line) shell
CMD bash
