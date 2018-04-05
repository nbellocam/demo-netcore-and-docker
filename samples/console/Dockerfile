FROM microsoft/dotnet:2.0-runtime
WORKDIR /app

# copy and build everything else
COPY ./demo/ ./
ENTRYPOINT ["dotnet", "console.dll"]