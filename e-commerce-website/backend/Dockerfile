# ---------- Build Stage ----------
FROM node:18-alpine AS build

WORKDIR /app

# Copy only package files first (better caching)
COPY package*.json ./

# Install only prod dependencies
RUN npm ci --only=production

# Copy application source
COPY . .


# ---------- Runtime Stage ----------
FROM node:18-alpine

WORKDIR /app

# Create non-root user (security best practice)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy built app
COPY --from=build /app /app

# Change ownership
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8000

CMD ["npm", "start"]
