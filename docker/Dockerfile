FROM debian:wheezy

RUN apt-get update && apt-get install -y --no-install-recommends\
    git python libav-tools apache2 php5 curl ca-certificates\
 && rm -rf /var/lib/apt/lists/*

#Install youtube-dl
RUN curl -L https://yt-dl.org/downloads/2016.08.01/youtube-dl -o /usr/local/bin/youtube-dl && chmod a+x /usr/local/bin/youtube-dl

WORKDIR /
RUN git clone https://github.com/mastqe/youtube-dl-webui.git /www/youtube-dl

WORKDIR /www/youtube-dl
RUN rm -rf .git README.md img .gitignore docker

WORKDIR /
RUN chmod -R 755 /www && chown -R www-data:www-data /www

COPY ./vhost.conf /etc/apache2/conf.d/extra/vhost.conf

RUN ln -sf /dev/stdout /var/log/apache2/youtube-dl_access.log
RUN ln -sf /dev/stderr /var/log/apache2/youtube-dl_error.log

EXPOSE 80

VOLUME /www/youtube-dl/downloads

CMD youtube-dl -U && /usr/sbin/apache2ctl -D FOREGROUND
