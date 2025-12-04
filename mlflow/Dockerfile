FROM python:3.11-slim-buster
RUN apt-get update && apt-get install -y \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install python packages
COPY requirements.txt /tmp
RUN pip install -r /tmp/requirements.txt
