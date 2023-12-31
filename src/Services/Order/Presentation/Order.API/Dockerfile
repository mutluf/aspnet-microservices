#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Services/Order/Presentation/Order.API/Order.API.csproj", "Services/Order/Presentation/Order.API/"]
COPY ["BuildingBlocks/EventBus.Messages/EventBus.Messages.csproj", "BuildingBlocks/EventBus.Messages/"]
COPY ["Services/Order/Core/Order.Application/Order.Application.csproj", "Services/Order/Core/Order.Application/"]
COPY ["Services/Order/Core/Order.Domain/Order.Domain.csproj", "Services/Order/Core/Order.Domain/"]
COPY ["Services/Order/Infrastructure/Order.Infrastructure/Order.Infrastructure.csproj", "Services/Order/Infrastructure/Order.Infrastructure/"]
COPY ["Services/Order/Infrastructure/Order.Persistence/Order.Persistence.csproj", "Services/Order/Infrastructure/Order.Persistence/"]
RUN dotnet restore "Services/Order/Presentation/Order.API/Order.API.csproj"
COPY . .
WORKDIR "/src/Services/Order/Presentation/Order.API"
RUN dotnet build "Order.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Order.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Order.API.dll"]