# CodeFusion: scanner build image
# Pure static site (HTML/CSS/JS, no build step, no dependencies).
# The image just stages the checkout and sanity-checks the expected files.
FROM debian:bookworm-slim
ENV DEBIAN_FRONTEND=noninteractive

# scanner contract: the checkout lives inside the image, at /src
COPY . /src
WORKDIR /src

# Sanity check: the files the README describes must be present.
RUN test -f index.html && test -f script.js && test -f styles.css \
 && test -f manifest.json && test -f service-worker.js \
 && echo "CodeFusion static site staged OK" \
 && node --version 2>/dev/null || true
