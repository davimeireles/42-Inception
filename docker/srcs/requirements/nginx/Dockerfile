FROM debian:bullseye

# Argumentos para configuração do certificado SSL
ARG CRED_PATH
ARG CRED_CERT
ARG CRED_KEY
ARG COUNTRY 
ARG STATE
ARG LOCALITY
ARG ORGANIZATION
ARG ORG_UNIT
ARG COMMON_NAME

# Instalação de dependências e limpeza
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y nginx openssl gettext-base && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Criação do diretório para certificados e geração do certificado SSL
RUN mkdir -p ${CRED_PATH} && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout ${CRED_PATH}/${CRED_KEY} \
    -out ${CRED_PATH}/${CRED_CERT} \
    -subj "/C=${COUNTRY}/ST=${STATE}/L=${LOCALITY}/O=${ORGANIZATION}/OU=${ORG_UNIT}/CN=${COMMON_NAME}"

	# Cópia e configuração do arquivo nginx.conf
COPY conf/nginx.conf /tmp/nginx.conf
RUN envsubst '$CRED_PATH $CRED_KEY $CRED_CERT $COMMON_NAME' < /tmp/nginx.conf > /etc/nginx/sites-available/default && \
    rm /tmp/nginx.conf

# Exposição da porta 443 para HTTPS, apenas para informações
EXPOSE 443

# Comando para iniciar o Nginx
ENTRYPOINT ["nginx", "-g", "daemon off;"]