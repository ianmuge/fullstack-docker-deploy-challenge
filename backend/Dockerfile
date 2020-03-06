FROM node:slim
LABEL maintainer="Ian Muge <ian.muge@gmail.com>"

RUN mkdir -p /app
WORKDIR /app
RUN chown -R node:node /app
COPY package*.json ./
USER node
RUN npm install --silent
COPY --chown=node:node . .
RUN chown -R node:node /app
EXPOSE 9000
ENTRYPOINT ["npm"]
CMD ["start"]