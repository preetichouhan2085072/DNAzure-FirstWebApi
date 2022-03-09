#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["Cloud-Day7_2ndHandsOn/Cloud-Day7_2ndHandsOn.csproj", "Cloud-Day7_2ndHandsOn/"]
RUN dotnet restore "Cloud-Day7_2ndHandsOn/Cloud-Day7_2ndHandsOn.csproj"
COPY . .
WORKDIR "/src/Cloud-Day7_2ndHandsOn"
RUN dotnet build "Cloud-Day7_2ndHandsOn.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Cloud-Day7_2ndHandsOn.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Cloud-Day7_2ndHandsOn.dll"]

FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env

WORKDIR /app

# Copy csproj and restore as distinct layers

COPY *.csproj ./

RUN dotnet restore

# Copy everything else and build

COPY . ./

RUN dotnet publish -c Release -o out

# Build runtime image

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2

WORKDIR /app

COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "Cloud-Day7_2ndHandsOn.dll"]