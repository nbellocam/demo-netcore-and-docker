# demo-netcore-and-docker

This repository contains a simple demo script and some samples to show .NET Core running using Docker.

If you want to see the slides that I used along with this demos you can fin it here: https://www.slideshare.net/nbellocam/introduccin-a-net-core-y-docker

## Dotnet demos

### Console

1. `dotnet new -h`

1. `dotnet new console`

1. `code .`

1. `dotnet run`


### MVC

1. `dotnet new -h`

1. `dotnet new console`

1. `code .`

1. `dotnet run`


## Docker

### Console + docker

https://docs.microsoft.com/en-us/dotnet/core/docker/docker-basics-dotnet-core

1. Add `Dockerfile`

    ```
    FROM microsoft/dotnet:2.0-sdk
    WORKDIR /app

    # copy csproj and restore as distinct layers
    COPY *.csproj ./
    RUN dotnet restore

    # copy and build everything else
    COPY . ./
    RUN dotnet publish -c Release -o out
    ENTRYPOINT ["dotnet", "out/console.dll"]
    ```

1. `docker ps`

1. `docker images`

1. `docker build -t dotnetapp-dev .`

1. `docker run --rm dotnetapp-dev`

### Console + docker no SDK

1. `dotnet publish -c Release -o demo`

1. Update `Dockerfile`

    ```
    FROM microsoft/dotnet:2.0-runtime
    WORKDIR /app

    # copy and build everything else
    COPY ./demo/ ./
    ENTRYPOINT ["dotnet", "console.dll"]
    ```

1. `docker build -t dotnetapp-runtime .`

1. `docker run --rm dotnetapp-runtime`

### Web + docker

1. En mvc agregar `Dockerfile`

    ```
    FROM microsoft/aspnetcore-build:2.0 AS build-env
    WORKDIR /app

    # Copy csproj and restore as distinct layers
    COPY *.csproj ./
    RUN dotnet restore

    # Copy everything else and build
    COPY . ./
    RUN dotnet publish -c Release -o out

    # Build runtime image
    FROM microsoft/aspnetcore:2.0
    WORKDIR /app
    COPY --from=build-env /app/out .
    ENTRYPOINT ["dotnet", "mvc.dll"]
    ```

1. `docker build -t webapp .`

1. `docker run -d -p 8080:80 --name myapp webapp`