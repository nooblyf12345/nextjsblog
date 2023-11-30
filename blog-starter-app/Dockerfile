# FROM node:18


# WORKDIR /usr/src/app

# COPY package*.json ./

# RUN npm install

# COPY . .

# EXPOSE 3000
# CMD npm run dev








FROM node:18-alpine as base
RUN apk add --no-cache g++ make py3-pip libc6-compat
WORKDIR /usr/src/app
COPY package*.json ./
EXPOSE 3000

FROM base as builder
WORKDIR /usr/src/app
COPY . .
RUN npm install 
RUN npm run build


FROM base as production
WORKDIR /usr/src/app

ENV NODE_ENV=production
RUN npm ci

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001
USER nextjs


COPY --from=builder --chown=nextjs:nodejs /usr/src/app/.next ./.next
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/package.json ./package.json
COPY --from=builder /usr/src/app/public ./public

CMD npm start

# FROM base as dev
# ENV NODE_ENV=development
# RUN npm install 
# COPY . .
# CMD npm run dev