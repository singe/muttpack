# Builder
FROM alpine:latest as builder
RUN apk update
RUN apk add build-base autoconf automake libidn2-dev ncurses-dev openssl-dev bzip2-dev cyrus-sasl-dev krb5-dev zlib-dev git kyotocabinet-dev gpgme-dev
RUN git clone --depth 1 https://gitlab.com/muttmua/mutt.git
# Make exploitation harder
ENV CFLAGS "-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -fPIE -fstack-clash-protection"
ENV CPPFLAGS "-Wdate-time -D_FORTIFY_SOURCE=3"
ENV CXXFLAGS "-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -fPIE -fstack-clash-protection"
ENV LDFLAGS "-Wl,-z,now -Wl,-z,relro -Wl,-pie"
# Remove a bunch of features since other programs will handle those
RUN cd mutt && ./prepare --disable-dependency-tracking --disable-warnings --disable-debug --enable-gpgme --enable-hcache --disable-imap --disable-pop --enable-sidebar --disable-smtp --without-gss --without-zlib --with-idn2 --without-sasl --without-ssl && make

# Runner
FROM alpine:latest
RUN apk update
RUN apk add w3m w3m-image curl msmtp bash vim jq mutt kyotocabinet openssl urlview
COPY --from=builder /mutt/mutt /usr/local/bin/
COPY --from=builder /mutt/doc/Muttrc /usr/local/etc/
COPY --from=builder /mutt/mutt_pgpring /usr/local/bin/
COPY --from=builder /mutt/muttbug /usr/local/bin/
COPY --from=builder /mutt/pgpewrap /usr/local/bin/
COPY --from=builder /mutt/smime_keys /usr/local/bin/
COPY --from=builder /mutt/flea /usr/local/bin/
RUN curl -o /usr/local/bin/msmtpq https://raw.githubusercontent.com/marlam/msmtp/master/scripts/msmtpq/msmtpq \
&& curl -o /usr/local/bin/msmtp-queue https://raw.githubusercontent.com/marlam/msmtp/master/scripts/msmtpq/msmtp-queue \
&& chmod +x /usr/local/bin/msmtp*

# Configs
RUN mkdir /root/.mutt
COPY muttrc /root/.mutt/muttrc
COPY mailcap /root/.mailcap
RUN mkdir /root/.w3m
COPY w3m/config /root/.w3m/config
COPY msmtprc /root/.msmtprc
COPY vimrc /root/.vimrc

# Mail dir (bind mount from host)
RUN mkdir -p /root/.mail

ENV EDITOR=vim
ENTRYPOINT [ "mutt" ]
