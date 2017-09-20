FROM node:0.12.7

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY . /usr/src/app
RUN npm install 
RUN npm link body-parser
RUN npm link mongoose

EXPOSE 3000

CMD ["npm", "start"]
