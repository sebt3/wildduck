FROM node:lts-alpine as builder
ARG version=3.4.0
ARG config_version=setup-v1.0.2
RUN apk add --no-cache git python3 make g++
WORKDIR /config
RUN git clone --depth 1 --branch ${config_version} https://github.com/nodemailer/wildduck-dockerized.git ./
WORKDIR /app
RUN git clone --depth 1 https://github.com/zone-eu/zone-mta-template ./
RUN npm install zone-mta@${version}
RUN npm install --production
RUN npm install zonemta-wildduck
RUN rm -rf .git config && mkdir -p config && cp -Rapvf /config/default-config/zone-mta/* /app/config/

FROM node:lts-alpine as app
ENV NODE_ENV production
RUN apk add --no-cache tini
WORKDIR /app
COPY --from=builder /app /app
ENTRYPOINT ["/sbin/tini", "--", "node", "index.js"]
CMD ["--config=config/zonemta.toml"]