FROM nginx:1.16.1

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -yq \
    vim

RUN usermod -u 1000 www-data

EXPOSE 80 443