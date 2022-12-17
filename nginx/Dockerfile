FROM nginx:latest

ARG SERVER_NAME $SERVER_NAME
ARG PROXY_PASS $PROXY_PASS

COPY ./default.conf.template /etc/nginx/conf.d/default.conf.template
RUN envsubst '${SERVER_NAME} ${PROXY_PASS}' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf

EXPOSE 80
