FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 5012

ENV ASPNETCORE_URLS=http://+:5012

FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG configuration=Release
WORKDIR /src
COPY ["ToDoListApp/ToDoListApp.csproj", "ToDoListApp/"]
RUN dotnet restore "ToDoListApp\ToDoListApp.csproj"
COPY . .
WORKDIR "/src/ToDoListApp"
RUN dotnet build "ToDoListApp.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "ToDoListApp.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ToDoListApp.dll"]
