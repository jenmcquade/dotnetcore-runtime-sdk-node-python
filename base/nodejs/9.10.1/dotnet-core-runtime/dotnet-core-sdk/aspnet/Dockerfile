### ASP.NET Environment with .NET Core 
FROM jonmcquade/dotnetcore-runtime-sdk-node-python:base-nodejs-9.10.1-dotnet-core-runtime-2.10-preview1-26216-03-dotnet-core-sdk-2.1.300-preview1-008174
ARG aspenv="Development"
ENV ASPNETCORE_ENVIRONMENT $aspenv
ARG serverUrls="http://+:80"
ENV ASPNETCORE_URLS $serverUrls
RUN apk add --update --no-cache libuv \
    && ln -s /usr/lib/libuv.so.1 /usr/lib/libuv.so \
    && mkdir /tmp/warmup \
    && cd /tmp/warmup \
    && dotnet new web --no-restore \
    && dotnet restore -p:RuntimeIdentifier=alpine.3.6-x64 \
        --source https://api.nuget.org/v3/index.json \
    && cd / \
    && rm -rf /tmp/warmup
EXPOSE 80