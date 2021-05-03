FROM alpine

RUN apk add nginx; \
    mkdir /run/nginx/; \
    echo "<h1>Hello world!</h1>" > /var/www/localhost/htdocs/index.html;

ADD $PWD/default.conf /etc/nginx/conf.d/default.conf
ADD $PWD/*.key /etc/ssl/private/
ADD $PWD/*.crt /etc/ssl/certs/

WORKDIR /var/www/localhost/htdocs

COPY $PWD/entrypoint.sh /usr/local/bin

ENTRYPOINT ["/bin/sh", "/usr/local/bin/entrypoint.sh"]

EXPOSE 80
EXPOSE 443

CMD ["/bin/sh", "-c", "nginx -g 'daemon off;'; nginx -s reload;"]
