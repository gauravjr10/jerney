# # ---- Build stage ----
# FROM node:20-alpine AS build
# WORKDIR /app
# COPY package.json package-lock.json* ./
# RUN npm ci --only=production && npm cache clean --force

# # ---- Production stage ----
# FROM node:20-alpine AS production

# # Security: run as non-root user
# RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# # Security: remove unnecessary packages
# RUN apk --no-cache add dumb-init

# WORKDIR /app

# # Copy only production dependencies from build stage
# COPY --from=build /app/node_modules ./node_modules
# COPY src/ ./src/

# # Security: own files by non-root user
# RUN chown -R appuser:appgroup /app

# USER appuser

# EXPOSE 5000

# # Security: use dumb-init for proper signal handling (PID 1 problem)
# ENTRYPOINT ["dumb-init", "--"]
# CMD ["node", "src/index.js"]
 
# FROM node:20-alpine AS build
# WORKDIR /app

# COPY package.json package-lock.json* ./
# RUN npm ci --only=production && npm cache clean --force

# FROM node:20-alpine AS production

# RUN addgroup -S addgroup && adduser -S adduser -G addgroup
# RUN apk --no-cache add dumb-init

# WORKDIR /app

# COPY --from=build /app/node_modules ./node_modules
# COPY src/ ./src/

# USER appuser
#  EXPOSE 5000

#  ENTRYPOINT ["dumb-init", "--"]
#  CMD ["node", "src/index.js"]
FROM node:20-alpine AS build
WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm ci --omit=dev && npm cache clean --force


FROM node:20-alpine AS production

RUN apk --no-cache add dumb-init

WORKDIR /app

# create correct user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build /app/node_modules ./node_modules
COPY src/ ./src/

# fix ownership (important)
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 5000

ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "src/index.js"]