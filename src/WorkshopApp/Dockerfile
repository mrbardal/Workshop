#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:latest AS base
USER app
WORKDIR /app
EXPOSE 8080
# EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:latest AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
# COPY ["WorkshopApp.csproj", "."]
COPY . .
RUN dotnet restore "./WorkshopApp.csproj"
WORKDIR "/src/."
RUN dotnet build "./WorkshopApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WorkshopApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WorkshopApp.dll"]
