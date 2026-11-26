FROM node:22-bookworm-slim

WORKDIR /app

# Install OpenSSL required by Prisma
RUN apt-get update \
    && apt-get install -y openssl \
    && rm -rf /var/lib/apt/lists/*

COPY package.json package-lock.json ./
COPY prisma ./prisma
COPY shared ./shared
RUN npm ci --omit=dev
RUN npx prisma generate

COPY AgentsService/package.json AgentsService/package-lock.json ./AgentsService/
RUN cd AgentsService && npm ci --omit=dev

COPY AgentsService/src ./AgentsService/src
# Reject malformed route/source files during image build, before Docker can
# replace a healthy Agents service with a container that crashes on startup.
RUN find AgentsService/src -type f -name '*.js' -print0 | xargs -0 -r -n1 node --check
RUN mkdir -p AgentsService/uploads
RUN chown -R node:node /app

WORKDIR /app/AgentsService
EXPOSE 3002
ENV NODE_ENV=production
USER node
CMD ["npm", "start"]
