FROM monooso/docker-php:latest

# Install Node. Sorry, Docker puritans.
# @see https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash

RUN apt-get update && apt-get install -y --fix-missing --no-install-recommends \
    build-essential \
    nodejs

# Install Yarn, for the cool kids.
RUN npm install -g yarn
