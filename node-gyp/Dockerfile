FROM node:lts-alpine

# install python, git and node-gyp
RUN apk --no-cache add --virtual native-deps \
  g++ gcc libgcc libstdc++ linux-headers make && \
  apk --no-cache add python git && \
  npm config set unsafe-perm true && \
  npm install --quiet node-gyp -g && \
  apk del native-deps
