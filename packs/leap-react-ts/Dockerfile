FROM node:10-slim
ENV PORT 8080
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN npm run build
CMD ["npm", "run", "serve"]