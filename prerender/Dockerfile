# https://hub.docker.com/_/node/tags
# https://github.com/tvanro/prerender-alpine/blob/master/Dockerfile
FROM node:23.11.0-alpine3.21

ENV CHROME_BIN=/usr/bin/chromium-browser
ENV CHROME_PATH=/usr/lib/chromium/
ENV MEMORY_CACHE=0
# install chromium and tini, clear cache
RUN apk add --no-cache chromium tini \
 && rm -rf /var/cache/apk/* /tmp/*

USER node
WORKDIR /prerender
COPY package*.json .
RUN npm ci --omit=dev && npm cache clean --force
COPY . .
EXPOSE 3000
ENTRYPOINT ["tini", "--"]
CMD ["node", "server.js"]
