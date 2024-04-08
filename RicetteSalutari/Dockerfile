# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ricettesalutari/ricettesalutari.csproj", "ricettesalutari/"]
RUN dotnet restore "./ricettesalutari/ricettesalutari.csproj"
COPY . .
WORKDIR "/src/ricettesalutari"
RUN dotnet build "./ricettesalutari.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ricettesalutari.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ricettesalutari.dll"]