# ---- Build Stage ----
FROM node:20-alpine AS base
WORKDIR /app    

# Copy dependency manifests first (layer caching)
COPY package*.json ./

# Copy source and compile TypeScript
FROM base AS dev
RUN npm install

COPY . . 
CMD ["npm", "run", "dev"]


FROM base as build
RUN npm ci
COPY . .
RUN npm run build



# ---- Production Stage ----
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --omit=dev

# Copy compiled JS from builder
COPY --from=build /app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/index.js"]