# Stage 1
FROM node:21 AS backend-builder 

# setup the wroking dir
WORKDIR /app

#code
COPY . .

RUN npm i

# Stage 2 
FROM node:21-slim

# setup the wroking dir
WORKDIR /app

#copy the above stage as compressed
COPY --from=backend-builder /app .

COPY .env.docker .env

#Port
EXPOSE 5000

CMD ["npm","start"]


