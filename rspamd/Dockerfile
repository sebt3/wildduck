ARG version=3.18.3
FROM alpine:${version}
RUN apk add --no-cache tini rspamd rspamd-controller rspamd-proxy rspamd-fuzzy ca-certificates curl \
 && ln -sf /proc/self/fd/1 /var/log/rspamd/rspamd.log
EXPOSE 11332/tcp 11333/tcp 11334/tcp
ENTRYPOINT ["/sbin/tini", "--", "/usr/bin/rspamd"]
CMD ["-i", "-f"]