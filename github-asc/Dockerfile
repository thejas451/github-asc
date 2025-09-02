# FROM windows-based:
# FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
# FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build

#  Use Linux-based instead:
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["github-asc.csproj", "."]
RUN dotnet restore "github-asc.csproj"
COPY . .
RUN dotnet build "github-asc.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "github-asc.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "github-asc.dll"]