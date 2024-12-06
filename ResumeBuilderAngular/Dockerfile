FROM node:18
WORKDIR /app
COPY . .
RUN npm install -f
EXPOSE 4200
CMD ["/bin/sh", "-c", "npm run generate-proxy-config && npm start"]