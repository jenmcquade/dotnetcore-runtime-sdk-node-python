# Alpine Node 9.10.1
## Implementing:
### Python and yarn
FROM node:9.10.1-alpine
WORKDIR /
RUN apk add --update \
    python \
    python-dev \
    py-pip \
    build-base \
		yarn \
  && pip install virtualenv \
  && rm -rf /var/cache/apk/* 