# Swagger加载慢

如果您的 .NET 6 Web 应用程序的 Swagger 页面加载速度很慢，可能是因为接口太多，导致 Swagger UI 需要加载大量的数据，从而导致卡顿。

1. 使用分组：可以将接口分为不同的组，并在 Swagger UI 上创建不同的选项卡，以便用户可以选择不同的选项卡查看接口。这可以减少 Swagger UI 需要加载的数据量，从而加快加载速度。您可以使用 Swashbuckle.AspNetCore 的分组功能来实现这一点。
2. 缩小扫描范围：在 Swagger UI 中显示的所有 API 都是从程序集中自动扫描的。如果您有许多程序集和命名空间，可以通过在 Swagger 配置文件中添加 IgnoreObsoleteActions 选项来忽略不需要显示的 API，从而减少扫描的范围。
3. 启用缓存：如果您的应用程序生成的 Swagger 文档不频繁更改，则可以启用 Swagger 缓存以减少文档生成时间。您可以在 Swagger 配置文件中添加 UseSwaggerUI(options => options.CacheEntries = 10) 选项来启用缓存。这会将 Swagger UI 中的缓存条目设置为 10，可以根据需要进行调整。
4. 开启压缩：在 Swagger 配置文件中启用压缩选项，可以通过减小数据大小来提高 Swagger UI 的加载速度。您可以使用 Microsoft.AspNetCore.ResponseCompression 包中提供的 ResponseCompression 中间件来实现这一点。