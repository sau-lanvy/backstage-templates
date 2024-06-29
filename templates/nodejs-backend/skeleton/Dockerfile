FROM node:20-slim as builder

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json package-lock.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:20-slim

ENV NODE_ENV production
USER node

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json package-lock.json ./

RUN npm ci --production

COPY --from=builder /usr/src/app/dist ./dist

ENV PORT ${{ values.port }}
EXPOSE ${{ values.port }}

CMD ["npm", "run", "start"]
