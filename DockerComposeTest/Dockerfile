#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
## Install curl. Need to check if server is running
RUN apt-get update && apt-get install -y \
	curl

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["DockerComposeTest/DockerComposeTest.csproj", "DockerComposeTest/"]
RUN dotnet restore "DockerComposeTest/DockerComposeTest.csproj"
COPY . .
WORKDIR "/src/DockerComposeTest"
RUN dotnet build "DockerComposeTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DockerComposeTest.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerComposeTest.dll"]