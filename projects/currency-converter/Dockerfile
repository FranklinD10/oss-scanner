# currency-converter: scanner build image
# React + Vite + Tailwind currency converter. Build is `vite build`.
FROM node:20-bookworm
ENV DEBIAN_FRONTEND=noninteractive

# scanner contract: the checkout lives inside the image, at /src
COPY . /src
WORKDIR /src

# Install dependencies exactly as locked, then build the way the maintainer builds it.
RUN npm ci --no-audit --no-fund \
 && npm run build

# Lint is the project's cheap static check; do not fail the image on it.
RUN (npx eslint . || echo "WARNING: eslint reported issues") 2>&1 | tail -5
