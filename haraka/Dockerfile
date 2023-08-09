FROM node:lts-alpine as builder
ARG version=3.0.2
ARG plugin_version=1.37.9
ARG config_version=setup-v1.0.2
RUN apk add --no-cache git python3 make g++
WORKDIR /config
RUN git clone --depth 1 --branch ${config_version} https://github.com/nodemailer/wildduck-dockerized.git ./
WORKDIR /app
RUN git clone --depth 1 --branch v${version} https://github.com/haraka/Haraka.git ./
RUN npm install --production
RUN npm install haraka-plugin-wildduck@${plugin_version}
RUN rm -rf .git config && mkdir -p config && cp -Rapvf /config/default-config/haraka/* /app/config/

FROM node:lts-alpine as app
ENV NODE_ENV production
RUN apk add --no-cache tini
WORKDIR /app
COPY --from=builder /app /app
ENTRYPOINT ["/sbin/tini", "--", "node", "haraka.js"]
