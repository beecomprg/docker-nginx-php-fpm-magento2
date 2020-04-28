# Magento 2 docker nginx with php fpm
- built from Linux Alpine

```
docker build -t beecomprg/docker-nginx-php-fpm-magento2:7.3.17-stretch ./7.3/fpm/stretch
docker push beecomprg/docker-nginx-php-fpm-magento2:7.3.17-stretch
docker build -t beecomprg/docker-nginx-php-fpm-magento2:7.2.30-stretch ./7.2/fpm/stretch
docker push beecomprg/docker-nginx-php-fpm-magento2:7.2.30-stretch
```
