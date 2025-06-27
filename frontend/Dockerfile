# Use an official Node.js runtime as a parent image
FROM node:20-alpine

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to leverage Docker cache
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# The main command to run when the container starts
# The --host 0.0.0.0 is important to expose the app outside the container
CMD ["ng", "serve", "--host", "0.0.0.0"]