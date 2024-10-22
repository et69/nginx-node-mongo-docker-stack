ARG NODE_VERSION=18

FROM node:${NODE_VERSION}-alpine

WORKDIR /app

COPY package.json .
RUN npm install 

COPY . .
RUN chown -R node:node ./

USER node

CMD sleep 20 && node app.js

