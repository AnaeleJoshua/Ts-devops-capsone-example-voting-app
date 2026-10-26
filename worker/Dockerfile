# Build stage
FROM --platform=${BUILDPLATFORM} mcr.microsoft.com/dotnet/sdk:9.0.319-bookworm-slim-amd64 AS build

ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM

RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

WORKDIR /source

COPY *.csproj .
RUN dotnet restore -a $TARGETARCH

COPY . .
RUN dotnet publish -c release -o /app -a $TARGETARCH --self-contained false --no-restore


# Runtime stage
FROM mcr.microsoft.com/dotnet/runtime:9.0 AS final

WORKDIR /app

COPY --from=build --chown=$APP_UID:$APP_UID /app .

# Run the application as the built-in non-root user
USER $APP_UID

ENTRYPOINT ["dotnet", "Worker.dll"]