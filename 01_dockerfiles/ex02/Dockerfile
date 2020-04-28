FROM ruby

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update && apt-get install -y nodejs yarn && rm -rf /var/lib/apt/lists/*
RUN gem install rails bundle

COPY app /opt/app
WORKDIR /opt/app

ONBUILD RUN yarn install --check-files
ONBUILD RUN bundle update --bundler
ONBUILD RUN bundle install
ONBUILD RUN rails db:migrate
ONBUILD RUN rails db:seed
