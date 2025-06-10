FROM node:latest AS build
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build 


FROM nginx:alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --chown=appuser:appgroup nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build --chown=appuser:appgroup /app/dist /usr/share/nginx/html
RUN touch /var/run/nginx.pid && \
    chown -R appuser:appgroup /var/run/nginx.pid &&\
    chown -R appuser:appgroup /var/cache/nginx && \
    chown -R appuser:appgroup /var/log/nginx && \
    chown -R appuser:appgroup /etc/nginx/conf.d
USER appuser

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]