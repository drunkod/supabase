## USAGE:
# Build:        docker build --target production -t supabase/studio:latest .
# Run:          docker run -p 3000:3000 supabase/studio
# Deploy:       docker push supabase/studio:latest
# Clean build:
#    docker build --target production --no-cache -t supabase/studio:latest .
#    docker builder prune


# FROM node:14 as base
# WORKDIR /usr/src/app
# # Do `npm ci` separately so we can cache `node_modules`
# # https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
# COPY package*.json ./
# RUN npm clean-install
# COPY . .
#STAGE 1
FROM node:12.7-alpine AS build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm clean-install
COPY . .
RUN npm run build

#STAGE 2
FROM nginx:1.17.1-alpine as base
COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build /usr/src/app/studio /usr/share/nginx/html
FROM base as dev
EXPOSE 8082
CMD ["npm", "run", "dev"]

FROM base as production
RUN npm run build
EXPOSE 3000
CMD ["npm", "run", "start"]
