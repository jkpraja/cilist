FROM node:lts-gallium

ENV NODE_ENV=development

WORKDIR /app

COPY . .

RUN apt update && apt install nano -y

RUN npm install

CMD [ "npm", "start"]