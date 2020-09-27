FROM node:13.12.0-alpine AS BUILD
COPY package.json /tmp
WORKDIR /tmp/
RUN npm install

FROM node:13.12.0-alpine

RUN mkdir /app
COPY --from=BUILD /tmp/ /app/
COPY src /app/src/

EXPOSE 3000
WORKDIR /app

CMD [ "npm", "start" ]