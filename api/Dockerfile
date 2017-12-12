# Build Stage
FROM microsoft/aspnetcore-build:2 AS build-env
WORKDIR /api

COPY api.csproj .
RUN dotnet restore
RUN dotnet --version
COPY . .

# set up the runtime store
RUN for version in '2.0.0' '2.0.3'; do \
        curl -o /tmp/runtimestore.tar.gz https://dist.asp.net/runtimestore/$version/linux-x64/aspnetcore.runtimestore.tar.gz \
        && export DOTNET_HOME=$(dirname $(readlink $(which dotnet))) \
        && tar -x -C $DOTNET_HOME -f /tmp/runtimestore.tar.gz \
        && rm /tmp/runtimestore.tar.gz; \
    done
    
RUN dotnet --version
RUN dotnet publish -o /publish

# Runtime Image Stage
FROM microsoft/aspnetcore:2
WORKDIR /publish
COPY --from=build-env /publish .
ENTRYPOINT ["dotnet", "api.dll"]