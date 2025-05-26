# Stage 1: Build 
FROM node:18-alpine AS builder

WORKDIR /app

#Copies && install dependencies in build
COPY package*.json ./

RUN npm install

#Copies source code && builds
COPY . .

RUN npm run build

# Stage 2: Runtime Production 
FROM node:18-alpine AS runner

# Set working directory
WORKDIR /app

# Set environment to production
ENV NODE_ENV=production

# Copy only necessary files from the builder
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/next.config.js ./next.config.js
COPY --from=builder /app/next-i18next.config.js ./next-i18next.config.js

# Install only production dependencies
RUN npm install --omit=dev

# Expose the port your app runs on
EXPOSE 3000

# Command to run the application
CMD ["npm", "start"]
