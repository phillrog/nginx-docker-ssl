FROM node:18-alpine3.15 AS build

RUN npm install -g @angular/cli
ADD ./portal/package.json /app/package.json
WORKDIR /app/
RUN npm install
ADD ./portal /app/
RUN ng build

FROM nginx:alpine

COPY --from=build /app/dist/portal /usr/share/nginx/html