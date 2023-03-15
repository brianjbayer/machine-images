# Dockerfile for a containerized Rails development environment
# ASSUMPTION: source is volume mounted
# docker build --no-cache -t brianjbayer/debian-11-ruby-3.2-rails-7.0.4-dev .
# docker run -it --rm -v $(pwd):/app -p 3000:3000 brianjbayer/debian-11-ruby-3.2-rails-7.0.4-dev
ARG BASE_IMAGE=ruby:3.2.1-slim-bullseye
FROM ${BASE_IMAGE}

ARG BUNDLER_VER=2.4.8
ARG RAILS_VER=7.0.4.3

# Install Build Packages, Bundler, and rails
ARG BUILD_PACKAGES='build-essential libpq-dev libsqlite3-dev'
RUN apt-get update \
  && apt-get -y dist-upgrade \
  && apt-get -y install ${BUILD_PACKAGES} \
  && rm -rf /var/lib/apt/lists/* \
  # Update gem command to latest
  && gem update --system \
  # install bundler and rails versions
  && gem install bundler:$BUNDLER_VER rails:$RAILS_VER

# --- Dev Environment ---
WORKDIR /app

# Add git, vim, and curl and postgresql-client (psql) at least
ARG DEVENV_PACKAGES='git vim curl postgresql-client'
RUN apt-get update \
  && apt-get dist-upgrade \
  && apt-get install -y ${DEVENV_PACKAGES} \
  && rm -rf /var/lib/apt/lists/*

# Add console IRB config
RUN echo "IRB.conf[:PROMPT_MODE] = :SIMPLE" >> ~/.irbrc \
  && echo "IRB.conf[:AUTO_INDENT_MODE] = false" >> ~/.irbrc

# Start devenv in (command line) shell
CMD bash
