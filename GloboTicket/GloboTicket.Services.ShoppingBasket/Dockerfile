FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
# RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
# USER appuser

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["GloboTicket.Services.ShoppingBasket/GloboTicket.Services.ShoppingBasket.csproj", "GloboTicket.Services.ShoppingBasket/"]
RUN dotnet restore "GloboTicket.Services.ShoppingBasket/GloboTicket.Services.ShoppingBasket.csproj"
COPY . .
WORKDIR "/src/GloboTicket.Services.ShoppingBasket"
RUN dotnet build "GloboTicket.Services.ShoppingBasket.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GloboTicket.Services.ShoppingBasket.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GloboTicket.Services.ShoppingBasket.dll"]