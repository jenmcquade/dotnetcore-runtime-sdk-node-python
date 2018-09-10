# Alpine Node 10.10
## Implementing:
### Python and yarn
FROM node:10.10-alpine
WORKDIR /
RUN apk add --update \
    python \
    python-dev \
    py-pip \
    build-base \
		yarn \
  && pip install virtualenv \
  && rm -rf /var/cache/apk/* 
