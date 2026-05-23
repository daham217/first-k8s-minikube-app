FROM nginx:alpine

COPY web/index.html /usr/share/nginx/html/index.html
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
