FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /app
# restore as distinct layers
COPY src/Api/*.csproj ./src/Api/
WORKDIR /app/src/Api
RUN dotnet restore
# publish
WORKDIR /app/
COPY src/Api/. ./src/Api/
WORKDIR /app/src/Api
RUN dotnet publish -c Release -o out


FROM build AS test
WORKDIR /app/test
COPY test/. .
WORKDIR /app/test/unit/Api
ENTRYPOINT ["dotnet", "test", "--logger:trx"]


FROM microsoft/dotnet:2.1-aspnetcore-runtime AS run
WORKDIR /app
COPY --from=build /app/src/Api/out ./
ENV ASPNETCORE_URLS="http://*:80"
EXPOSE 80/tcp
ENTRYPOINT ["dotnet", "myapp.dll"]
