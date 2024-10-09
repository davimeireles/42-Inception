FROM debian:bullseye

# Instalação de dependências
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y \
        php7.4-fpm \
        php7.4-mysqli \
        curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Configuração de diretórios e permissões
RUN mkdir -p /run/php /var/www/html/wp-content/uploads && \
    chown -R www-data:www-data /run/php /var/www/html

# Cópia de arquivos de configuração
COPY conf/www.conf /etc/php/7.4/fpm/pool.d/
COPY wp-install.sh .

# Configuração de permissões para o script de instalação
RUN chmod -x wp-install.sh

# Exposição da porta do PHP-FPM
EXPOSE 9000

# Comando para iniciar o script de instalação
ENTRYPOINT ["./wp-install.sh"]