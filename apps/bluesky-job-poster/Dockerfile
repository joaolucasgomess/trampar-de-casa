# Base Stage
FROM node:18-alpine AS base

WORKDIR /app

ARG BSKY_EMAIL
ARG BSKY_PASSWORD
ARG SUPABASE_URL
ARG SUPABASE_SERVICE_ROLE

ENV BSKY_EMAIL=$BSKY_EMAIL \
    BSKY_PASSWORD=$BSKY_PASSWORD \
    SUPABASE_URL=$SUPABASE_URL \
    SUPABASE_SERVICE_ROLE=$SUPABASE_SERVICE_ROLE

# Dependencies Stage
FROM base AS dependencies

WORKDIR /app

COPY apps/bluesky-job-poster ./apps/bluesky-job-poster
COPY packages ./packages
COPY package*.json turbo.json yarn.lock ./

RUN apk add --no-cache python3 build-base

RUN yarn install

# Final Stage (Runner)
FROM base AS runner

WORKDIR /app

COPY --from=dependencies /app /app

RUN npm install -g ts-node

WORKDIR /app/apps/bluesky-job-poster

CMD ["ts-node", "src/index.ts"]