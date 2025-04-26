FROM alpine:latest
RUN apk update
RUN apk add msmtp bash curl coreutils grep
RUN curl -o /usr/local/bin/msmtpq https://raw.githubusercontent.com/marlam/msmtp/master/scripts/msmtpq/msmtpq
RUN curl -o /usr/local/bin/msmtp-queue https://raw.githubusercontent.com/marlam/msmtp/master/scripts/msmtpq/msmtp-queue
RUN chmod +x /usr/local/bin/msmtp*

# Configs
COPY msmtprc /root/.msmtprc

# Suppress msmtp connectivity testing since we're in an isolated container
ENV EMAIL_CONN_TEST x

CMD msmtp-queue -r
