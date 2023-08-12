FROM node:lts-alpine as builder
ARG UID=1000
ARG version=1.0.1
ENV APP_PATH /app
USER root
RUN apk add --no-cache git python3 make g++ openssl
USER node
WORKDIR ${APP_PATH}
RUN git clone --depth 1 --branch ${version} https://github.com/nodemailer/wildduck-webmail.git ./
RUN rm -rf .git Dockerfile README.md .github
RUN npm install && npm run bowerdeps

FROM node:lts-alpine as app
ENV NODE_ENV production
RUN apk add --no-cache openssl
ENV APP_PATH /app
WORKDIR ${APP_PATH}
COPY --from=builder ${APP_PATH}/ ${APP_PATH}/
COPY --from=builder ${APP_PATH}/config/default.toml /etc/wildduck/www.toml
ENTRYPOINT ["node", "server.js"]
CMD ["--config=/etc/wildduck/www.toml"]
