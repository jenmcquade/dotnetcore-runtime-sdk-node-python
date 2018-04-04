# Alpine Node 8.1.1 (LTS)
## Implementing:
### Python and yarn
FROM node:8.1.1-alpine
WORKDIR /
RUN apk add --update \
    python \
    python-dev \
    py-pip \
    build-base \
		yarn \
  && pip install virtualenv \
  && rm -rf /var/cache/apk/* 