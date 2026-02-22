
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:10.0 AS restore
WORKDIR /src
COPY ["Directory.Build.props", "."]
COPY ["Directory.Packages.props", "."]
COPY ["DevHabit.slnx", "."]
COPY ["DevHabit.Api/DevHabit.Api.csproj", "DevHabit.Api/"]
RUN dotnet restore DevHabit.slnx --packages ./artifacts/pkg --artifacts-path ./artifacts

FROM restore AS build
ARG BUILD_CONFIGURATION=Release
COPY [".", "."]
RUN dotnet build DevHabit.slnx --no-restore --configuration $BUILD_CONFIGURATION --artifacts-path ./artifacts

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish DevHabit.slnx --no-build --configuration $BUILD_CONFIGURATION --artifacts-path ./artifacts


FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081
COPY --from=publish /src/artifacts/publish/DevHabit.Api/release .
ENTRYPOINT ["dotnet", "DevHabit.Api.dll"]

