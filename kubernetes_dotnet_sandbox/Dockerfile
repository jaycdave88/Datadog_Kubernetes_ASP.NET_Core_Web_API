FROM microsoft/dotnet:sdk AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM microsoft/dotnet:aspnetcore-runtime

# Datadog agent start
RUN apt-get update && apt-get install -y bash curl

RUN mkdir -p /opt/datadog
RUN curl -LO https://github.com/DataDog/dd-trace-dotnet/releases/download/v0.7.0-beta/datadog-dotnet-apm_0.7.0_amd64.deb
RUN apt install ./datadog-dotnet-apm_0.7.0_amd64.deb

ENV CORECLR_ENABLE_PROFILING=1
ENV CORECLR_PROFILER={846F5F1C-F9AE-4B07-969E-05C26BC060D8}
ENV CORECLR_PROFILER_PATH=/opt/datadog/Datadog.Trace.ClrProfiler.Native.so
ENV DD_INTEGRATIONS=/opt/datadog/integrations.json

# Datadog agent end

WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "kubernetes_dotnet_sandbox.dll"]