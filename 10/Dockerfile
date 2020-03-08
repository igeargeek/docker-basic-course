FROM node:alpine3.10
WORKDIR /src/app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]

EXPOSE 3000