FROM nginx:1.16.1

RUN apt-get update && apt-get install -y \
    vim

RUN usermod -u 1000 www-data

EXPOSE 80 443