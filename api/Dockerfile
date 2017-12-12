# Build Stage
FROM microsoft/aspnetcore-build:2 AS build-env
WORKDIR /api

COPY api.csproj .
RUN dotnet restore
RUN dotnet --version
COPY . .
RUN dotnet --version
RUN dotnet publish -o /publish

# Runtime Image Stage
FROM microsoft/aspnetcore:2
WORKDIR /publish
COPY --from=build-env /publish .
ENTRYPOINT ["dotnet", "api.dll"]