FROM node:lts-alpine as builder
ARG version=3.0.2
ARG plugin_version=1.37.9
RUN apk add --no-cache git python make g++
WORKDIR /app
RUN git clone --depth 1 --branch ${version} https://github.com/haraka/Haraka.git ./
RUN npm install --production
RUN npm install haraka-plugin-wildduck@${plugin_version}

FROM node:lts-alpine as app
ENV NODE_ENV production
RUN apk add --no-cache tini
WORKDIR /app
COPY --from=builder /app /app
ENTRYPOINT ["/sbin/tini", "--", "node", "haraka.js"]