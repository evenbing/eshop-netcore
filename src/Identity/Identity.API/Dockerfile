#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/Identity/Identity.API/Identity.API.csproj", "src/Identity/Identity.API/"]
COPY ["src/Identity/Identity.Application/Identity.Application.csproj", "src/Identity/Identity.Application/"]
COPY ["src/Identity/Identity.Core/Identity.Core.csproj", "src/Identity/Identity.Core/"]
COPY ["src/Identity/Identity.Infrastructure/Identity.Infrastructure.csproj", "src/Identity/Identity.Infrastructure/"]
RUN dotnet restore "src/Identity/Identity.API/Identity.API.csproj"
COPY . .
WORKDIR "/src/src/Identity/Identity.API"
RUN dotnet build "Identity.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Identity.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Identity.API.dll"]
