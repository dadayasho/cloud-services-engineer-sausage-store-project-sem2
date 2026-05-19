
FROM node:10.15.2-alpine AS builder
WORKDIR /app
COPY . ./
RUN npm install
RUN npm run build


FROM nginx:1.30-alpine AS runner
RUN rm /usr/share/nginx/html/*
COPY --from=builder /app/dist /usr/share/nginx/html
COPY --from=builder /app/nginx.tmpl /etc/nginx/templates/default.conf.template
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]