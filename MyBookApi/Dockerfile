# Base image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy only the csproj file first to restore dependencies
COPY ["./MyBookApi.csproj", "MyBookApi/"]
RUN dotnet restore "MyBookApi/MyBookApi.csproj"

# Copy the remaining source code files
COPY . .

# Set working directory to the project folder
WORKDIR "/src/MyBookApi"
RUN dotnet build "MyBookApi.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "MyBookApi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyBookApi.dll"]
