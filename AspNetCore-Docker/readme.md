# ASP.NET Core Docker YAML

This YAML first builds an ASP.NET Core application (file artifacts) and copies those files into a Docker container to build an image.

## Sample Docker File

### Linux

```yaml
# escape=`

FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base # TODO: update this to your specific ASP.NET Core version
WORKDIR /app

EXPOSE 80

FROM microsoft/dotnet:2.2-sdk AS build # TODO: update this to your specific ASP.NET Core version
WORKDIR /app
COPY _artifacts/dockercontents .

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 `
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINERS=true `
    # ignore first time expierence
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE="true"

ENTRYPOINT ["dotnet", "MyAspNetCoreApp.dll"]

```

### Windows
```yaml
# escape=`

FROM microsoft/dotnet-framework:4.7.2-runtime-windowsservercore-1803 # TODO: update this to your specific .NET Framework / Windows version
WORKDIR /app
COPY _artifacts/dockercontents .

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 `
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINERS=true `
    # ignore first time expierence
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE="true"

ENTRYPOINT ["MyAspNetCoreApp.exe"]
```

