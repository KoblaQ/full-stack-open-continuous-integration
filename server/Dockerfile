FROM node:25-alpine AS build-stage

WORKDIR /usr/src/app

# Copy packages and install for both client and server
COPY client/package*.json ./client/
COPY server/package*.json ./server/
RUN npm ci --prefix client
RUN npm ci --prefix server

COPY client ./client
COPY server ./server

RUN npm run build --prefix client
RUN rm -rf server/dist && cp -r client/dist server/

RUN npm run test --prefix server


# Production stage 
FROM node:25-alpine AS production

WORKDIR /usr/src/app/server

ENV NODE_ENV=production


COPY --from=build-stage /usr/src/app/server ./

CMD ["npm", "start"]