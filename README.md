# Docker: How to create and run a docker image from a . NET 8 Web API application

## 0. Prerequisites

Install Docker Desktop

Create a new account in Docker Hub

Install Visual Studio 2022 Community edition

## 1. Create a .NET 8 Web API in Visual Studio 2022 community

**IMPORTANT**! Install and **Run Docker Desktop** before starting creating the application in Visual Studio 2022

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/c6444f00-a64e-4fc3-a515-db8bf7e1b66e)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/f87185c0-9051-41cb-89cd-f5a547144bb9)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/d005f758-c124-4483-bd35-5a00cb4faa51)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/2af5048d-66c5-4159-b728-0bbf9082d0e3)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/a4fa4f42-0952-453c-a04e-02aaf8716333)

This is the dockefile source code

```dockerfile
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebAPIdotNET8.csproj", "."]
RUN dotnet restore "./././WebAPIdotNET8.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./WebAPIdotNET8.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WebAPIdotNET8.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAPIdotNET8.dll"]
```

## 2. Create the dockerfile

For creating automatically a dockerfile we add "docker support" in our application



## 3. Create the application Docker image

```
docker build -t dockerreponame/dockerimagename:tag .
```

For example:

```
docker build -t luiscoco/webapi:latest
```


## 4. Upload/download the Docker image to/from the Docker Hub





## 5. Run the docker image





