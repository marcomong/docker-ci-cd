# base image
FROM node:12.2.0-alpine as base
# port exposed
EXPOSE 8081
# Node env
ENV NODE_ENV=production
# set working directory
WORKDIR /app
# install and cache app dependencies
COPY package.json /app/package.json
RUN npm install --only=production


####
# DEVELOPMENT STAGE
FROM base as dev
# Node env
ENV NODE_ENV=development
# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
# set working directory
WORKDIR /app
# don't need to copy the cose because we are binding it
RUN npm install --only=development
CMD ["nodemon", "./app.js", "--inspect=0.0.0.0:9929"]

FROM base as test
# Node env
ENV NODE_ENV=development
# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
# set working directory
WORKDIR /app
# don't need to copy the cose because we are binding it
RUN npm install --only=development
CMD ["npm", "run", "test"]


#########
#PROD
FROM base as prod
WORKDIR /app
COPY . .
CMD ["npm", "run", "start"]