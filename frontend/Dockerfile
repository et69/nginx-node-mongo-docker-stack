ARG NGINX_VERSION=1.23
FROM nginx:${NGINX_VERSION}-alpine

COPY index.html style.css /usr/share/nginx/html/

COPY default.conf /etc/nginx/conf.d/

EXPOSE 80
CMD ["nginx","-g","daemon off;"]
