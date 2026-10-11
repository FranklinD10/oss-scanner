# SchoolGo: scanner build image
# Python Flask chatbot app. Pinned requirements target Python <= 3.10
# (numpy 1.22.0 has no 3.11 wheels), so we build on 3.10.
FROM python:3.10-bookworm
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=1

# scanner contract: the checkout lives inside the image, at /src
COPY . /src
WORKDIR /src

# Install the pinned dependencies the way the maintainer pins them.
RUN pip install --upgrade pip \
 && pip install -r requirements.txt

# Compile-check every Python file so syntax errors fail the build loudly.
# (We do NOT import app.py here: it requires OPENAI_API_KEY at import time.)
RUN python -m compileall -q app.py config.py
