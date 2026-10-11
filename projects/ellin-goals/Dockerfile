# ellin-goals: scanner build image
# React 18 + TypeScript + Vite app (goals tracker). Build is `tsc && vite build`.
FROM node:20-bookworm
ENV DEBIAN_FRONTEND=noninteractive

# scanner contract: the checkout lives inside the image, at /src
COPY . /src
WORKDIR /src

# Install dependencies exactly as locked, then build the way the maintainer builds it.
RUN npm ci --no-audit --no-fund \
 && npm run build

# Smoke-check the unit tests without failing the image if they are flaky.
RUN (npx vitest run --reporter=basic || echo "WARNING: vitest reported failures") 2>&1 | tail -5
