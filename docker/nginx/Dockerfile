FROM nginx:stable AS dev



FROM nginx:stable as prod

WORKDIR /app

COPY ./app/public ./public

COPY ./docker/nginx/files /