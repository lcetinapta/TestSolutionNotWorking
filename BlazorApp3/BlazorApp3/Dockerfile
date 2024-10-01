FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorApp3/BlazorApp3/BlazorApp3.csproj", "BlazorApp3/BlazorApp3/"]
COPY ["BlazorApp3/BlazorApp3.Client/BlazorApp3.Client.csproj", "BlazorApp3/BlazorApp3.Client/"]
RUN dotnet restore "BlazorApp3/BlazorApp3/BlazorApp3.csproj"
COPY . .
WORKDIR "/src/BlazorApp3/BlazorApp3"
RUN dotnet build "BlazorApp3.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "BlazorApp3.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorApp3.dll"]
