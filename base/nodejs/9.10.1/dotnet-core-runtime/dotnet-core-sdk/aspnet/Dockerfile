### ASP.NET Environment with .NET Core 
FROM jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-sdk
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