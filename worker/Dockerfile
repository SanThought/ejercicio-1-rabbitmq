FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install --omit=dev
COPY worker.js .
CMD ["node", "worker.js"]
