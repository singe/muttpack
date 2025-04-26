FROM alpine:latest
RUN apk update
RUN apk add openjdk17-jre-headless curl unzip
#RUN mkdir davmail && curl -L -o davmail/davmail.zip https://sourceforge.net/projects/davmail/files/davmail/6.2.2/davmail-6.2.2-3546.zip/download && unzip -d davmail/ davmail/davmail.zip
RUN mkdir davmail && curl -L -o davmail/davmail.zip https://sourceforge.net/projects/davmail/files/davmail/6.3.0/davmail-6.3.0-3627.zip/download && unzip -d davmail/ davmail/davmail.zip
COPY davmail.properties davmail/.davmail.properties
RUN sed -i "s/davmail.allowRemote=false/davmail.allowRemote=true/" davmail/.davmail.properties
RUN sed -i "s/davmail.bindAddress=.*/davmail.bindAddress=0.0.0.0/" davmail/.davmail.properties
#RUN echo "davmail.server=true" >> davmail/.davmail.properties
#RUN sed -i "s/127.0.0.1\tlocalhost/127.0.0.1\tlocalhost office365.microsoft.com/" /etc/hosts

# 1025 SMTP
# 1080 CalDav
# 1143 IMAP
# 1389 LDAP
EXPOSE 1025/tcp
EXPOSE 1080/tcp
EXPOSE 1143/tcp
EXPOSE 1389/tcp

ENTRYPOINT [ "davmail/davmail", "davmail/.davmail.properties", "-notray" ]
