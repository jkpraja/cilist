FROM node:lts-gallium

WORKDIR /app

COPY . .

RUN apt update && apt install nano -y

RUN npm install

CMD ["npm","start"]