# Blazor介绍

* Blazor server

  >Blazor Server项目模板：`blazorserver`
  >
  >Blazor Server 模板创建 Blazor Server 应用的初始文件和目录结构。 该应用中填充了一个 `FetchData` 组件的演示代码，该组件从注册服务 `WeatherForecastService` 加载数据，且用户与 `Counter` 组件交互。
  >
  >- `Data` 文件夹：包含 `WeatherForecast` 类和 `WeatherForecastService` 的实现，它们向应用的 `FetchData` 组件提供示例天气数据。
  >
  >- `Pages` 文件夹：包含构成 Blazor 应用的可路由组件/页面 (`.razor`) 和 Blazor Server 应用的根 Razor 页面。 每个页面的路由都是使用 [`@page`](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#page) 指令指定的。 该模板包括以下组件：
  >
  >  - ```
  >    _Host.cshtml
  >    ```
  >
  >    ：实现为 Razor 页面的应用的根页面：
  >
  >    - 最初请求应用的任何页面时，都会呈现此页面并在响应中返回。
  >    - 此主机页面指定根 `App` 组件 (`App.razor`) 的呈现位置。
  >
  >  - `Counter` 组件 (`Counter.razor`)：实现“计数器”页面。
  >
  >  - `Error` 组件 (`Error.razor`)：当应用中发生未经处理的异常时呈现。
  >
  >  - `FetchData` 组件 (`FetchData.razor`)：实现“提取数据”页面。
  >
  >  - `Index` 组件 (`Index.razor`)：实现 Home 页。
  >
  > 备注
  >
  >添加到 `Pages/_Host.cshtml` 文件的 JavaScript (JS) 文件应显示在关闭 `</body>` 标记之前。 在某些情况下，从 JS 文件加载自定义 JS 代码的顺序非常重要。 例如，确保在 Blazor framework JS 文件之前包含带有互操作方法的 JS 文件。
  >
  >- `Properties/launchSettings.json`：保留[开发环境配置](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/environments?view=aspnetcore-5.0#development-and-launchsettingsjson)。
  >
  >- ```
  >  Shared
  >  ```
  >
  >   
  >
  >  文件夹：包含以下共享组件和样式表：
  >
  >  - `MainLayout` 组件 (`MainLayout.razor`)：应用的[布局组件](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/components/layouts?view=aspnetcore-5.0)。
  >  - `MainLayout.razor.css`：应用主布局的样式表。
  >  - `NavMenu` 组件 (`NavMenu.razor`)：实现边栏导航。 包括 [`NavLink`](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/fundamentals/routing?view=aspnetcore-5.0#navlink-and-navmenu-components) 组件 ([NavLink](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.navlink))，该组件可向其他 Razor 组件呈现导航链接。 [NavLink](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.navlink) 组件会在系统加载其组件时自动指示选定状态，这有助于用户了解当前显示的组件。
  >  - `NavMenu.razor.css`：应用导航菜单的样式表。
  >  - `SurveyPrompt` 组件 (`SurveyPrompt.razor`)：Blazor 调查组件。
  >
  >- `wwwroot`：应用的 [Web 根目录](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/?view=aspnetcore-5.0#web-root)文件夹，其中包含应用的公共静态资产。
  >
  >- `_Imports.razor`：包括要包含在应用组件 (`.razor`) 中的常见 Razor 指令，如用于命名空间的 [`@using`](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#using) 指令。
  >
  >- `App.razor`：应用的根组件，用于使用 [Router](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.router) 组件来设置客户端路由。 [Router](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.router) 组件会截获浏览器导航并呈现与请求的地址匹配的页面。
  >
  >- `appsettings.json` 和环境应用设置文件：提供应用的[配置设置](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/fundamentals/configuration?view=aspnetcore-5.0)。
  >
  >- `Program.cs`：应用的入口点，用于设置 ASP.NET Core [主机](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-5.0)。
  >
  >- `Startup.cs`：包含应用的启动逻辑。 `Startup` 类定义两个方法：
  >
  >  - `ConfigureServices`：配置应用的[依赖关系注入 (DI)](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0) 服务。 通过调用 [AddServerSideBlazor](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.extensions.dependencyinjection.componentservicecollectionextensions.addserversideblazor) 添加服务，并将 `WeatherForecastService` 添加到服务容器以供示例 `FetchData` 组件使用。
  >
  >  - ```
  >    Configure
  >    ```
  >
  >    ：配置应用的请求处理管道：
  >
  >    - 调用 [MapBlazorHub](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.componentendpointroutebuilderextensions.mapblazorhub) 可以为与浏览器的实时连接设置终结点。 使用 [SignalR](https://docs.microsoft.com/zh-cn/aspnet/core/signalr/introduction?view=aspnetcore-5.0) 创建连接，该框架用于向应用添加实时 Web 功能。
  >    - 调用 [`MapFallbackToPage("/_Host")`](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.razorpagesendpointroutebuilderextensions.mapfallbacktopage) 以设置应用的根页面 (`Pages/_Host.cshtml`) 并启用导航。

* Blazer WebAssembly

  >Blazor WebAssembly项目模板：`blazorwasm`
  >
  >Blazor WebAssembly 模板创建 Blazor WebAssembly 应用的初始文件和目录结构。 该应用中填充了一个 `FetchData` 组件的演示代码，该组件从静态资产 `weather.json` 加载数据，且用户与 `Counter` 组件交互。
  >
  >- `Pages` 文件夹：包含构成 Blazor 应用的可路由组件/页面 (`.razor`)。 每个页面的路由都是使用 [`@page`](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#page) 指令指定的。 该模板包括以下组件：
  >  - `Counter` 组件 (`Counter.razor`)：实现“计数器”页面。
  >  - `FetchData` 组件 (`FetchData.razor`)：实现“提取数据”页面。
  >  - `Index` 组件 (`Index.razor`)：实现 Home 页。
  >- `Properties/launchSettings.json`：保留[开发环境配置](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/environments?view=aspnetcore-5.0#development-and-launchsettingsjson)。
  >
  >- ```
  >  Shared
  >  ```
  >
  >   
  >
  >  文件夹：包含以下共享组件和样式表：
  >
  >  - `MainLayout` 组件 (`MainLayout.razor`)：应用的[布局组件](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/components/layouts?view=aspnetcore-5.0)。
  >  - `MainLayout.razor.css`：应用主布局的样式表。
  >  - `NavMenu` 组件 (`NavMenu.razor`)：实现边栏导航。 包括 [`NavLink`](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/fundamentals/routing?view=aspnetcore-5.0#navlink-and-navmenu-components) 组件 ([NavLink](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.navlink))，该组件可向其他 Razor 组件呈现导航链接。 [NavLink](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.navlink) 组件会在系统加载其组件时自动指示选定状态，这有助于用户了解当前显示的组件。
  >  - `NavMenu.razor.css`：应用导航菜单的样式表。
  >  - `SurveyPrompt` 组件 (`SurveyPrompt.razor`)：Blazor 调查组件。
  >
  >- ```
  >  wwwroot
  >  ```
  >
  >  ：应用的
  >
  >   
  >
  >  Web 根目录
  >
  >  文件夹，其中包含应用的公共静态资产，包括
  >
  >   
  >
  >  ```
  >  appsettings.json
  >  ```
  >
  >   
  >
  >  和
  >
  >  配置设置
  >
  >  的环境应用设置文件。
  >
  >   
  >
  >  ```
  >  index.html
  >  ```
  >
  >   
  >
  >  网页是实现为 HTML 页面的应用的根页面：
  >
  >  - 最初请求应用的任何页面时，都会呈现此页面并在响应中返回。
  >  - 此页面指定根 `App` 组件的呈现位置。 使用 `app` 的 `id` (`<div id="app">Loading...</div>`) 在 `div` DOM 元素的位置呈现组件。
  >
  > 备注
  >
  >添加到 `wwwroot/index.html` 文件的 JavaScript (JS) 文件应显示在关闭 `</body>` 标记之前。 在某些情况下，从 JS 文件加载自定义 JS 代码的顺序非常重要。 例如，确保在 Blazor framework JS 文件之前包含带有互操作方法的 JS 文件。
  >
  >- `_Imports.razor`：包括要包含在应用组件 (`.razor`) 中的常见 Razor 指令，如用于命名空间的 [`@using`](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#using) 指令。
  >- `App.razor`：应用的根组件，用于使用 [Router](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.router) 组件来设置客户端路由。 [Router](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.components.routing.router) 组件会截获浏览器导航并呈现与请求的地址匹配的页面。
  >
  >- `Program.cs`：应用入口点，用于设置 WebAssembly 主机：
  >  - `App` 组件是应用的根组件。 对于根组件集合 (`builder.RootComponents.Add<App>("#app")`)，使用 `app` 的 `id`（`wwwroot/index.html` 中的 `<div id="app">Loading...</div>`）将 `App` 组件指定为 `div` DOM 元素。
  >  - 添加并配置了[服务](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/fundamentals/dependency-injection?view=aspnetcore-5.0)（例如，`builder.Services.AddSingleton<IMyDependency, MyDependency>()`）。