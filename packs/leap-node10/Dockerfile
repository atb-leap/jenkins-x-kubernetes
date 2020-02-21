FROM node:10-slim

ENV NODE_ENV=production
ENV PORT 8080

EXPOSE 8080

WORKDIR /usr/src/app

COPY package.json /usr/src/app
COPY yarn.lock /usr/src/app
RUN yarn install --production

COPY . .

CMD ["yarn", "start"]