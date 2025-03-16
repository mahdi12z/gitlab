# gitlab
# Setting Up and Developing Storybook with Docker Compose
## CI/CD
This document provides a step-by-step guide on how to set up and run Storybook inside a Docker container using Docker Compose for a streamlined development workflow.
### Prerequisites
1.Docker and Docker Compose must be installed on your system.

2.Node.js and npm or Yarn should be installed for local testing.  

# 1. Create and Configure Dockerfile.dev
Create a file named Dockerfile.dev in the project root and add the following content:
```bash

FROM node:20-alpine

WORKDIR /usr/src/app

COPY package.json ./
COPY package-lock.json ./
RUN npm install

COPY . /usr/src/app/

EXPOSE 6006

CMD ["npm", "run", "storybook"]


```


Explanation
. FROM node:20-alpine → Uses a lightweight Node.js Alpine image.

. WORKDIR /usr/src/app → Sets /usr/src/app/ as the working directory.

. COPY package.json ./ and COPY package-lock.json ./ → Copies dependency files.

. RUN npm install → Installs dependencies inside the container.

. COPY . /usr/src/app/ → Copies all project files to the container.

. EXPOSE 6006 → Opens port 6006 for Storybook.

. CMD ["npm", "run", "storybook"] → Runs Storybook automatically when the container starts.

# 2. Create and Configure docker-compose.dev.yml

Create a file named docker-compose.dev.yml in the project root and add the following content:
```bash


services:
  storybook:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - ./storybook:/usr/src/app:/usr/src/app
      - /usr/src/app/node_modules
    ports:
      - "6006:6006"
    environment:
      NODE_ENV: development
    command: ["npm", "run", "storybook"]

```


Explanation
. build.context: . → Uses the project directory as the build context.

. dockerfile: Dockerfile.dev → Specifies Dockerfile.dev for building the image.

. volumes:
./:/usr/src/app → Syncs code between the host and container.
/usr/src/app/node_modules → Keeps node_modules inside the container to avoid conflicts with host dependencies.

. ports: "6006:6006" → Exposes port 6006 for accessing Storybook.

command: ["npm", "run", "storybook"] → Runs Storybook automatically when the container starts.


# 3. Build and Run Storybook in Docker
## 1. Build the Docker image and start Storybook
```bash
docker compose -f docker-compose.dev.yml build
docker compose -f docker-compose.dev.yml up
```
## 2. Run Storybook in the background (Detached Mode)
```bash
docker compose -f docker-compose.dev.yml up -d
```
## 3. Access Storybook in your browser
```bash
http://localhost:6006
```




