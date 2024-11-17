# ------------ Stage 1: Build Stage ------------------
FROM node:21 AS frontend-builder

#set the wroking directory to /app
WORKDIR /app

#Copy the package.json and package-ock.json for dependency installation
COPY package*.json ./

#Install dependencies
RUN npm i

# Copy the rest of the apllication code
COPY . .

# ------------ Stage 2: Final Stage ------------------
FROM node:21-slim

#set the wroking directory to /app
WORKDIR /app

# copy built assest and dependencies from frontend-builder stage
COPY --from=frontend-builder /app .

# copy the .env.sample file to .env.local
COPY .env.sample .env.local

# Expose port 5173 for node.j application
EXPOSE 5173

CMD ["npm","run","dev","--","--host"]




