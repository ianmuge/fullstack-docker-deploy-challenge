FROM node:lts-alpine
LABEL maintainer="Ian Muge <ian.muge@gmail.com>"

ARG W_DIR=/home/node/app
RUN mkdir -p ${W_DIR}
RUN chown -R node:node ${W_DIR}

WORKDIR ${W_DIR}
COPY package*.json ./
USER node
RUN npm install --silent
COPY --chown=node:node . .
EXPOSE 8000
ENTRYPOINT ["npm"]
CMD ["start"]
