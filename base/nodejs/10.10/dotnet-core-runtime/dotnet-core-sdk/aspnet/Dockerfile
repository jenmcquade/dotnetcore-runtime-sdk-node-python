### ASP.NET Environment with .NET Core 
FROM jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-sdk
ARG aspenv="Development"
ENV ASPNETCORE_ENVIRONMENT $aspenv
ARG serverUrls="http://+:80"
ENV ASPNETCORE_URLS $serverUrls
# Install ASP.NET Core
ENV ASPNETCORE_VERSION 2.1.3

RUN apk add --no-cache --virtual .build-deps \
        openssl \
    && wget -O aspnetcore.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-linux-musl-x64.tar.gz \
    && aspnetcore_sha512='5699445c571a64c68000cf97555debee4439d892a43d3409c14dc730eca38b16dc8a4842807c3ed9b086d3e2e41fca28e15af430cbadf3c9959b055b17893795' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet \
    && rm aspnetcore.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && apk del .build-deps

RUN apk add --update --no-cache libuv \
    && ln -s /usr/lib/libuv.so.1 /usr/lib/libuv.so \
    && mkdir /tmp/warmup \
    && cd /tmp/warmup \
    && dotnet new web --no-restore \
    && dotnet restore -p:RuntimeIdentifier=alpine.3.7-x64 \
        --source https://api.nuget.org/v3/index.json \
    && cd / \
    && rm -rf /tmp/warmup
EXPOSE 80