# Build Stage
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS dotnet5angular-dotnetcorebuildstage
RUN apt-get update -y && apt-get install -y xz-utils && \
    wget https://nodejs.org/dist/v16.17.1/node-v16.17.1-linux-x64.tar.xz && \
    tar -xJf node-v16.17.1-linux-x64.tar.xz && \
    mv node-v16.17.1-linux-x64 /node && \
    rm -f node-v16.17.1-linux-x64.tar.xz
ENV PATH="$PATH:/node/bin"

WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c release --no-restore

# Run Stage
FROM mcr.microsoft.com/dotnet/aspnet:5.0
ENV DOTNET_GENERATE_ASPNET_CERTIFICATE=false
WORKDIR /app
USER 10014
EXPOSE 5001
EXPOSE 5000
ENV ASPNETCORE_URLS http://*:5001;http://*:5000

COPY --from=dotnet5angular-dotnetcorebuildstage /src/bin/Release/net5.0/publish ./
CMD ["dotnet", "dotnet5angular.dll"]
