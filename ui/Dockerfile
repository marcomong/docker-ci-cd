# base image
FROM node:12.2.0-alpine as base
# set working directory
WORKDIR /app
# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
# install and cache app dependencies
COPY package.json /app/package.json





### DEVELOPMENT
### dev
FROM base as dev
WORKDIR /app
RUN npm install
RUN npm install @vue/cli@3.7.0 -g
EXPOSE 8080
# start app
CMD ["npm", "run", "serve"]

### DEVELOPMENT
### dev
FROM base as test
WORKDIR /app
RUN npm install
RUN npm install @vue/cli@3.7.0 -g
# start app
CMD ["npm", "run", "test:unit"]



# build environment
FROM base as build
RUN npm install --silent --only=production
RUN npm install @vue/cli@3.7.0 -g
COPY . /app
RUN npm run build

## PRODUCTION
# production environment
FROM nginx:1.16.0-alpine as prod
COPY --from=build /app/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]