FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENV ASPNETCORE_ENVIRONMENT=Development

RUN apt update && apt-get -y upgrade && apt install -y tzdata && rm -rf /var/lib/apt/lists/*
# Set TimeZone in Debian
ENV TZ="America/Bogota"

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY [".", "."]
RUN dotnet restore "FrontEnd.csproj"
RUN dotnet build "FrontEnd.csproj" --configuration Release --output /app/build

FROM build AS publish
RUN dotnet publish "FrontEnd.csproj" -c Release -r linux-x64 --sc false -o /app/publish -p:PublishReadyToRun=true  

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FrontEnd.dll"]