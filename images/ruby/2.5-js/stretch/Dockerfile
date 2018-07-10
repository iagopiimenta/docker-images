FROM ruby:2.5.1

RUN apt-get update &&\
    apt-get install -y locales \
                       graphviz \
                       imagemagick &&\
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen &&\
    rm -rf /var/lib/apt/lists/*

RUN apt-get update &&\
    apt-get install -y postgresql-client-9.6 &&\
    rm -rf /var/lib/apt/lists/*

ARG NODE_DATE_INSTALL=20180710

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - &&\
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - &&\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update &&\
    apt-get install -y yarn \
                       nodejs &&\
    rm -rf /var/lib/apt/lists/*

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV APP_ROOT /app

RUN mkdir $APP_ROOT

WORKDIR $APP_ROOT

RUN groupadd -r app &&\
    groupmod -g 1000 app &&\
    useradd -g app -ms /bin/bash app &&\
    chown -R app:app $APP_ROOT

RUN mkdir -p /gems && chown 1000:1000 /gems
RUN mkdir -p /home/app

COPY .irbrc /home/app/.irbrc
RUN chown -R 1000:1000 /home/app/

USER app

ENV GEM_HOME /gems/vendor
ENV GEM_PATH /gems/vendor
ENV GEM_SPEC_CACHE /gems/specs
ENV BUNDLE_APP_CONFIG /gems/vendor
ENV BUNDLE_PATH /gems/vendor
ENV BUNDLE_BIN /gems/vendor/bin
ENV PATH /app/bin:/gems/vendor/bin:$PATH

VOLUME /gems
VOLUME /home/app
