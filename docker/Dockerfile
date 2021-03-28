FROM node:14-alpine

RUN apk update \
    && apk --no-cache add unzip curl \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && aws/install \
    && aws --version && docker -v
