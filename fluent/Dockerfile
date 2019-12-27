FROM fluent/fluentd:v1.7-debian-1

# Use root account to use apt
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish
RUN gem install fluent-plugin-elasticsearch 
RUN gem install fluent-plugin-http
RUN gem install fluent-plugin-tdlog
COPY ./fluent.conf /fluentd/etc/