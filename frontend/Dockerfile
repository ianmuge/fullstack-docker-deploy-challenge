FROM node:slim
LABEL maintainer="Ian Muge <ian.muge@gmail.com>"

RUN mkdir -p /app/node_modules
RUN chown -R node:node /app
WORKDIR /app
COPY package*.json ./
USER node
RUN npm install
COPY --chown=node:node . .
EXPOSE 8000
ENTRYPOINT ["npm"]
CMD ["start"]
