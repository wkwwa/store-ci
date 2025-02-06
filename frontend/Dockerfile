# build
FROM node:16 as builder
WORKDIR /app
COPY . .
RUN npm install
ENV VUE_APP_API_URL=/api
ENV NODE_ENV=production
RUN npm run build

#release
FROM nginx:latest
WORKDIR /app
ENV VUE_APP_API_URL=/api
ENV NODE_ENV=production
COPY default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist/ ./frontend/dist/momo-store
RUN ls -la ./frontend/dist/momo-store/*
EXPOSE 8081