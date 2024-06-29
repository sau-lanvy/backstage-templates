# Stage 1 - Install dependencies
FROM node:20-alpine AS deps

COPY package.json package-lock.json ./
RUN npm ci

# Stage 2 - Build the source code
FROM node:20-alpine AS builder

COPY . .
COPY --from=deps /opt/app-root/src/node_modules ./node_modules

USER root
RUN npm run build

# Stage 3 - Production image, copy all the files and start NodeJS
FROM node:20-alpine AS runner

COPY --from=deps --chown=1001:1001 /opt/app-root/src .
COPY --from=builder --chown=1001:1001 /opt/app-root/src/dist ./dist

ENV NODE_ENV production

# Switch to nodejs user
USER 1001

# Install production dependencies
RUN npm ci

ENV PORT ${{ values.port }}
EXPOSE ${{ values.port }}

CMD ["npm", "run", "start"]
