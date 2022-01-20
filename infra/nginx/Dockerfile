FROM nginx:1.16.1-alpine

RUN rm -rf /etc/nginx/conf.d
COPY /infra/nginx/conf.d/ /etc/nginx/conf.d/
COPY /backend /work
