# Elixir + Phoenix
FROM registry.svs.nonprod.uscis.dhs.gov/uscis_ubuntu:latest

ENV APP_HOME=/app

# Elixir requires UTF-8
RUN apt-get update && apt-get upgrade -y && apt-get install locales && locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install debian packages
RUN sudo apt-get install --yes build-essential inotify-tools postgresql-client wget curl

# Elixir install -- https://elixir-lang.org/install.html#unix-and-unix-like
RUN wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb \
    && sudo dpkg -i erlang-solutions_1.0_all.deb \
    && sudo apt-get update \
    && sudo apt-get install -y esl-erlang \
    && sudo apt-get install -y elixir
# https://blog.danielberkompas.com/2015/08/28/how-to-run-elixir-cloud9-ide/ from 2015 indicates we may need to touch couch.db
#   && sudo touch /etc/init.d/couchdb

#Copy the source folder into the Docker image
ADD . $APP_HOME
WORKDIR $APP_HOME

# Install Phoenix packages
ENV MIX_EXS=$APP_HOME/mix.exs
RUN echo $MIX_EXS

RUN mix local.hex --force
RUN mix local.rebar --force
RUN mix archive.install --force https://github.com/phoenixframework/archives/raw/master/phx_new.ez

# Install node
RUN curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
RUN sudo apt-get install -y nodejs

EXPOSE 4000
