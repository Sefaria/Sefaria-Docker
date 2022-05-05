FROM python:3.8

ENV PYTHONUNBUFFERED 1
ENV PYTHONPATH /www
ENV DJANGO_SETTINGS_MODULE sefaria.settings
ENV NVM_DIR /usr/local/nvm

RUN apt-get -y update \ 
  && apt-get -y upgrade \
  && apt-get install -y sqlite3 libsqlite3-dev gettext cmake ninja-build python3-dev cython3 pybind11-dev libre2-dev
#
# Install nvm with node and npm
RUN mkdir -p "$NVM_DIR" \
  && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
  && . $NVM_DIR/nvm.sh \
  && nvm install node

RUN mkdir /db \
 && /usr/bin/sqlite3 /db/db.sqlite

RUN mkdir /www \
 && mkdir /log

# This assumes that code is already in ./Sefaria-Project 
COPY ./Sefaria-Project /www/

WORKDIR /www

# install pyre2
RUN pip install cython pyre2

RUN pip install -r requirements.txt \
  && cp -R ./locale/en/LC_MESSAGES/ /usr/local/lib/python3.8/site-packages/django/conf/locale/en

# https://stackoverflow.com/questions/69665222/node-js-17-0-1-gatsby-error-digital-envelope-routinesunsupported-err-os
ENV NODE_OPTIONS=--openssl-legacy-provider
 
RUN . $NVM_DIR/nvm.sh \
  && npm install \
  && npm run build
