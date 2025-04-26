FROM alpine:latest
RUN apk update
RUN apk add isync

# Configs
COPY mbsyncrc /root/.mbsyncrc

# Mail dir (bind mount from host)
RUN mkdir -p ~/.mail

CMD mbsync -a
