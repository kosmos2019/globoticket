FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["GloboTicket.Gateway.MobileBff/GloboTicket.Gateway.MobileBff.csproj", "GloboTicket.Gateway.MobileBff/"]
RUN dotnet restore "GloboTicket.Gateway.MobileBff/GloboTicket.Gateway.MobileBff.csproj"
COPY . .
WORKDIR "/src/GloboTicket.Gateway.MobileBff"
RUN dotnet build "GloboTicket.Gateway.MobileBff.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GloboTicket.Gateway.MobileBff.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GloboTicket.Gateway.MobileBff.dll"]

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
#RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
#USER appuser