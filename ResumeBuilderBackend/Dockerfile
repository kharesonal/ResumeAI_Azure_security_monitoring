FROM node:18
WORKDIR /app
COPY package*.json ./
COPY . .
RUN npm install
RUN npm install -g typescript
RUN npm run build
RUN apt-get update && apt-get install -y chromium
COPY . .
EXPOSE 4292
CMD [ "npm", "start" ]