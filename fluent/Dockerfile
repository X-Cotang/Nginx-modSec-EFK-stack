FROM fluent/fluentd
# Use root account to use apt
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN gem install fluent-plugin-elasticsearch 
RUN gem install fluent-plugin-http
#RUN chmod -R 777 /home/log
RUN chmod -R 777 /var/log
COPY ./fluent.conf /fluentd/etc/