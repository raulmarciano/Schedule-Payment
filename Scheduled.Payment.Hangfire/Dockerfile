#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Scheduled.Payment.Hangfire/Scheduled.Payment.Hangfire.csproj", "Scheduled.Payment.Hangfire/"]
RUN dotnet restore "Scheduled.Payment.Hangfire/Scheduled.Payment.Hangfire.csproj"
COPY . .
WORKDIR "/src/Scheduled.Payment.Hangfire"
RUN dotnet build "Scheduled.Payment.Hangfire.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Scheduled.Payment.Hangfire.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Scheduled.Payment.Hangfire.dll"]