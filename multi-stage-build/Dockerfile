FROM fusionauth/fusionauth-app as build

FROM nginx

ARG SERVER_NAME $SERVER_NAME

COPY ./default.conf.template /etc/nginx/conf.d/default.conf.template
RUN envsubst '${SERVER_NAME}' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf

COPY --from=build /opt/openjdk /opt/openjdk
COPY --from=build /usr/local/fusionauth /usr/local/fusionauth

ARG DATABASE_HOST $DATABASE_HOST
ARG DATABASE_NAME $DATABASE_NAME
ARG DATABASE_PASSWORD $DATABASE_PASSWORD

RUN sed -i "s|localhost:5432/fusionauth|$DATABASE_HOST:5432/$DATABASE_NAME|" /usr/local/fusionauth/config/fusionauth.properties
RUN sed -i "s|database.password=fusionauth|database.password=$DATABASE_PASSWORD|" /usr/local/fusionauth/config/fusionauth.properties

ENV FUSIONAUTH_USE_GLOBAL_JAVA=1
ENV JAVA_HOME=/opt/openjdk
ENV PATH=$PATH:$JAVA_HOME/bin

EXPOSE 80

COPY ./startup.sh startup.sh
RUN chmod 755 startup.sh
CMD ["./startup.sh"]