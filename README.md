# Docker: How to create and run a docker image from a . NET 8 Web API application

## 0. Prerequisites

Install Docker Desktop

Create a new account in Docker Hub

Install Visual Studio 2022 Community edition

## 1. Create a .NET 8 Web API in Visual Studio 2022 community

**IMPORTANT**! Install and **Run Docker Desktop** before starting creating the application in Visual Studio 2022

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/7b58cdb3-a290-457a-b15d-76562a2da203)

Run Visual Studio 2022 Community Edition and create a new project 

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/c6444f00-a64e-4fc3-a515-db8bf7e1b66e)

Select the .NET Web API template

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/f87185c0-9051-41cb-89cd-f5a547144bb9)

Set the project name and location

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/d005f758-c124-4483-bd35-5a00cb4faa51)

Select the .NET 8 framework, also select **Enable Docker** for creating a dockerfile automatically

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/2af5048d-66c5-4159-b728-0bbf9082d0e3)

Click on the dockerfile to see the content

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

## 2. Create and run the Docker image 

We can automatically create and run the application Docker Image pressing the **Docker** button in Visual Studio 2022

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/ea68b43b-71c6-4dbc-adf9-f9edb7d0127b)

See the docker image in Docker Desktop

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/93ea102a-5e00-4857-910b-6ac32b3539e3)

See the docker running container in Docker Desktop

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/fa7fb04d-d25d-404d-88be-bc3fa2aed0c9)

Also we can see the image with the command prompt command:

```
docker image
```

And we can see the running containers with the command:

```
docker ps
```

Finally see the Web API running container endpoints

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/9d3a9601-78ce-4d29-8af8-dc04a5d53d5b)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/192dfb78-52b5-4673-a030-4fe4389b69c8)

**IMPORTANT NOTE**: 

Another option for creating automatically a dockerfile is to add "docker support" in our application

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/ee3340e7-90b0-428d-b796-1000853e8c9f)

Then we select the docker virtual machine operating system, in our case we select "**Linux**"

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/ab91a28d-faca-4be2-8dc2-542683523561)

## 3. Upload/download the Docker image to/from the Docker Hub

Login in Docker Hub and create a new account (Sign Up)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/26e2923e-e081-46f4-a64f-1dbe16ae58a6)

After creating an account we Sing in

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/9f85488e-a4e0-4f06-8b90-0f8973742f33)

Press the "**Create Repository**" button

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/b67d942c-8e60-4d1e-8c3d-0cb72b480799)

We set the new repo name, we select public or private repo and we include a repo description.

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/d0faa9c1-815f-4ba0-a90c-238c88a80a87)

We can Push images to this repo with the following commands: 

```
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

We can see the repositories list

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/7b88adb2-3b56-4cd6-a3fb-011eefbea51e)

## 4. Create the docker image and upload to the new repo in Docker Hub

This is the general syntax for creating a new image that we would like to upload to the Docker Hub repo. 

Pay attention we have to include the docker hub repo name then the docker image name and finally the docker image tag.

```
docker build -t dockerreponame/dockerimagename:tag .
```

For a real example, we right click on the project name and we select the menu option "**Open in Terminal**" then we run the following command: 

```
docker build -t luiscoco/webapidotnet8:latest .
```

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/c6ed928a-1553-440b-9c84-28d622d13706)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/d8e48865-36a0-4e6d-b690-85ac96e43788)

We check we create the docker image and we run the docker container 

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/582d4f54-57e2-4a05-9c88-e70cf79870cd)

Then we use the docker push command to upload the image to the Docker Hub repository:

```
docker push luiscoco/webapidotnet8:latest
```

Also we can pull the docker image from the Docker Hub repository with the following command:

```
docker pull luiscoco/webapidotnet8
```

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/376dc20d-4ae0-4aed-842c-5c71b5b2cd5a)

See the new docker image in the repo

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/7e38db3a-ed91-40b8-bfbd-e2cc333b9d17)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/9b4c6f9e-5b0b-45c6-bed3-e286cfbf3a94)

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/a1cb5201-7055-4d35-9fcd-51489592e49e)

## 5. We pull the docker image from Docker hub and we run in Docker Desktop

We pull the image with the command:

```
docker pull luiscoco/webapidotnet8
```

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/612b1aa1-8dc9-4ecc-b5cc-13eb2b9bb8d7)

Then we open Docker Desktop and we confirm the docker image was downloaded but it is not yet running in a container

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/3a6fabd4-a6b5-44f2-af43-1c619e815058)

To run the docker image in a container, we press the run button (see the following image), or we type this command

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/e60629a4-7321-48eb-b99f-fa9485622898)

Or we can type the following command, taking into account we map the launchSetting.json http port with the 8080 port in dockerfile

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/d504e779-9cbd-4a4d-9c31-d416d0b91974)

```
docker run -d -p 5269:8080 luiscoco/webapidotnet8:latest
```

And also we can see the docker logs, see this picture

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/5d67f6bb-d0db-43a8-b2a2-e9504a8fa745)

Now in Docker Desktop we can see the docker container is running

![image](https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API/assets/32194879/f98ec62a-6ec6-4192-9f1e-eddcb3d9b85a)

We can also navigate to the Web API endpoints:









