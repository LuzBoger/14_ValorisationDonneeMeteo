FROM dhi.io/node:24-dev AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM dhi.io/node:24 AS prod

WORKDIR /app

ENV NODE_ENV=production \
    HOST=0.0.0.0 \
    PORT=3000

COPY --from=build --chown=1000:1000 /app/.output /app/.output

USER 1000

EXPOSE 3000

ENTRYPOINT ["node"]
CMD [".output/server/index.mjs" ]
