# Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 
WORKDIR /app
EXPOSE 80/tcp
COPY dist /app
ENTRYPOINT ["dotnet", "TemplateApi.dll"]
```

先执行<code>dot net publish --output dist .</code>

