# dotnetcore-runtime-sdk-node-python
**.NET Toolkit Base Images**
* This GitHub repository builds [jonmcquade/dotnetcore-runtime-sdk-node-python](https://hub.docker.com/r/jonmcquade/dotnetcore-runtime-sdk-node-python/) Docker .NET Core tool images for Linux Alpine.
* **Progressive dependency management**: You can use any level of the image builds depending on the requirements of your project.  For example, you can run `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:base` to only use Node and Python dev tools, with no .NET runtime or SDK.  Or, you can run `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-runtime` to download an image with Node, Python, and the .NET Core Runtime, but no SDK.
* .NET Core SDK images include the .NET Core Runtime
* ASP .NET Core images include the SDK and Runtime

## Purpose
The built images are best as a base image for debugging, development, testing and publishing a .NET Core 2.1 app. The (:dotnetcore-asp) tagged image is being implemented in [this ASP.NET Core 2 Boilerplate](https://github.com/jonmcquade/aspnetcore-react-redux) 
## Building a self-contained .NET Core 2.1 app
There are dev tools built inside the image that can be run using:
* `docker run jonmcquade/dotnetcore-runtime-sdk-node-python [executable under /user/local/bin]`
For example, you can run this to run `npm install` in your working directory 
* Assuming you have a `package.json` file in your current directory:
* `docker run -v .:/MyProject jonmcquade/dotnetcore-runtime-sdk-node-python npm install` 

**From a global webpack installation** with a package.json file in your project (.) directory, 
you could install project dependencies into your shared volume
* `docker exec mysdk npm install`
* **After executing**, you should see a `node_modules` directory in your host machine's project.

You can also use Interactive Terminal to enter a running container
* `docker run --name mysdk ti jonmcquade/dotnetcore-runtime-sdk-node-python shell`

## Base: NodeJS Apline 3.6 with Python Dev tools and Yarn (:base)
[https://hub.docker.com/_/node/](https://github.com/nodejs/docker-node/blob/b3ca6573b5c179148b446107386ae96ac6204ad3/8/alpine/Dockerfile)
* Official NodeJS image with Alpine Linux for small image sizes
* `py` and `yarn` available inside containers
* `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:base`

### Python
* https://pkgs.alpinelinux.org/package/v3.3/main/x86/python
* https://pkgs.alpinelinux.org/package/v3.3/main/x86/python-dev

### Yarn
* https://pkgs.alpinelinux.org/package/edge/community/x86_64/yarn

## .NET Core Runtime (:dotnetcore-runtime) 
[2.1.0-preview1](https://www.microsoft.com/net/download/dotnet-core/runtime-2.1.0-preview1)
* .NET Core Runtime is built from the (:base) tagged images
* `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-runtime`

## .NET Core SDK (:dotnetcore-sdk)
* [2.1.300-preview1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)
* .NET Core SDK is built from the .NET Core Runtime (:dotnetcore-runtime) tagged image
* `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-sdk`

## ASP .NET Core (:dotnetcore-asp)
* ASP .NET configurations from the .NET Core SDK (:dotnetcore-sdk) tagged image
* A temp project is temporarily created during build to cache ASP .NET Core dependencies
* `docker pull jonmcquade/dotnetcore-runtime-sdk-node-python:dotnetcore-asp`




