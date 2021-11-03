FROM php:7.4-apache

# RUN , CMD , ENTRYPOINT , COPY , ADD を実行する時のワーキングディレクトリを指定
WORKDIR /var/www

RUN apt-get update

# パッケージインストール
RUN apt-get install -y libonig-dev libzip-dev unzip vim

# PHPのライブラリインストール
RUN docker-php-ext-install mbstring pdo_mysql

COPY ./php.ini /usr/local/etc/php/
COPY ./*.conf /etc/apache2/sites-available/
    
# Composerのインストール & パス通す
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"

RUN mv composer.phar /usr/local/bin/composer
RUN echo "export PATH=\$PATH:/root/.composer/vendor/bin" >> /root/.bashrc

# ComposerでLaravelのインストーラをインストールしておく
RUN composer global require laravel/installer


