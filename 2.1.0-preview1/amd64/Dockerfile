# Alpine .Net Core Runtime 2.1.0 
## Implementing:
### ASP.NET Core SDK 2.1.300-preview1-008174
### NodeJS 8.11.0, NPM, Yarn 1.5.1 and Python baked in
## This image allows full stack ASP .NET Core 2.1 development
##  with no local installation
FROM node:8.11-alpine as base
ARG dotNetVer="2.1.0-preview1-26216-03"
ENV DOTNET_VERSION $dotNetVer
WORKDIR /
RUN apk add --update \
    python \
    python-dev \
    py-pip \
    build-base \
		yarn \
  && pip install virtualenv \
  && rm -rf /var/cache/apk/* \
	&& apk add --no-cache \
        ca-certificates \
        \
        # .NET Core dependencies
        krb5-libs \
        libcurl \
        libgcc \
        libintl \
        libssl1.0 \
        libstdc++ \
        libunwind \
        libuuid \
        tzdata \
        userspace-rcu \
        zlib \
    && apk -X https://dl-cdn.alpinelinux.org/alpine/edge/main add --no-cache \
    	lttng-ust \
		# .NET Core Runtime 
		&& apk add --no-cache --virtual .build-deps \
	    openssl \
    && wget -O dotnet.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Runtime/$DOTNET_VERSION/dotnet-runtime-$DOTNET_VERSION-alpine.3.6-x64.tar.gz \
    && dotnet_sha512='2462480ccfb69308d61749122814de7b398e21eee952e597c7812c529a41394dad7411d6654f984f30f0e1c5e0ef8ce4749e247e12cc66bc49b69a1951d803f6' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && rm dotnet.tar.gz \
    && apk del .build-deps

##
#
# .NET SDK 2.1 builder 
#
##
FROM base as sdk
ARG aspenv="Development"
ENV ASPNETCORE_ENVIRONMENT $aspenv
ENV DOTNET_SDK_VERSION 2.1.300-preview1-008174
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT false
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
# Disable the invariant mode (set in base image)
RUN apk add --no-cache icu-libs libuv
RUN echo -e "\n\n\n...Installing .NET Core $DOTNET_SDK_VERSION ...\n\n\n" \
        && ln -s /usr/lib/libuv.so.1 /usr/lib/libuv.so \
				&& apk add --no-cache --virtual .build-deps openssl \
        && wget -O dotnet.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-alpine.3.6-x64.tar.gz \
        && dotnet_sha512='7705aebf88264a4a2e65c7fac29de92aa5e21d49709494d2e3242d89af6a65c1ae101f6b8e822c0a097b4d7d490cb76ee7c716f949101d5eed746f2c0f43097e' \
        && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
        && mkdir -p /usr/share/dotnet \
        && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
        && rm /usr/bin/dotnet \
        && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
        && rm dotnet.tar.gz \
        && apk del .build-deps
ENV NUGET_XMLDOC_MODE skip 

# Establish runtime env vars, expose ports, set CMD to entrypoint shell
## entrypoint uses .tpl files to rewrite host and port rules
FROM sdk AS final
ARG aspenv="Development"
ENV ASPNETCORE_ENVIRONMENT $aspenv
ARG port="5000"
ENV PORT $port
ARG ssl_port="5001"
ENV SSL_PORT $ssl_port
ARG serverUrls="http://*:$PORT;http://*:$SSL_PORT"
ENV ASPNETCORE_URLS $serverUrls
EXPOSE $port $ssl_port

CMD dotnet help