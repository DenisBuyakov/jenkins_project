FROM node:14-alpine

RUN apk update &&
apk add unzip &&
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" &&
unzip awscliv2.zip &&
sudo ./aws/install
