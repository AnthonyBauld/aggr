FROM node:20.2.0-alpine3.17 AS build

RUN apk add --no-cache git

ARG AGGR_REF=6c1d85d2802bddb7a32545548de7b56aeaf9e1f7
WORKDIR /usr/src/app

RUN git clone https://github.com/Tucsky/aggr.git . \
  && git checkout "$AGGR_REF"

COPY ./env.production.override ./.env.production

RUN npm ci
RUN npm run build -- --mode production

FROM nginx:stable-alpine

COPY --from=build /usr/src/app/dist /usr/share/nginx/html
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
