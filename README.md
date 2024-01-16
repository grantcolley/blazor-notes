# blazor-notes
> [!NOTE]
> This is a collection of notes about **ASP.NET Core Blazor** I have taken while reading other resources\*. The purpose of this `readme` is simply to consolidate them into a single page for my own reference.
>
> In some cases, the note may be a copy and paste. In others, I may shorten it as per my understanding of the original text. In all cases, the content is not my own and I provide a link to the original source\*.
>
> \* *this content is mostly from [**Microsoft** documentation](https://learn.microsoft.com/en-us/aspnet/core/blazor)*

## Table of Contents
* [Overview](#overview)
  * [Full-stack Web App](#full-stack-web-app)
  * [Supported Platforms](#supported-platforms)
  * [Hosting Models](#hosting-models) 
    * [Blazor Server hosting model](#blazor-server-hosting-model)
    * [Blazor WebAssembly](#blazor-webassembly)
    * [Blazor Hybrid](#blazor-hybrid)
* [Fundamentals](#fundamentals)
  * [Static and Interactive Rendering Concepts](#static-and-interactive-rendering-concepts)
  * [Routing and Navigation](#routing-and-navigation)
    * [Focus an Element on Navigation](#focus-an-element-on-navigation)
    * [Route to Components from Multiple Assemblies](#route-to-components-from-multiple-assemblies)
    * [Interactive-Routing](#interactive-routing)
    * [Route Parameters](#route-parameters)
    * [NavigationManager](#navigationmanager)
  * [Dependency Injection](#dependency-injection)
    * [Service Lifetime](#service-lifetime)
    * [Keyed Services](#keyed-services)
    * [OwningComponentBase](#owningcomponentbase)
  * [Startup](#startup)
    * [Startup Process](#startup-process)
    * [Load Client-side Boot Resources](#load-client-side-boot-resources)
    * [Control Headers in Code](#control-headers-in-code)
  * [Logging](#logging)
  * [Exceptions](#exceptions)
    * [Handle Caught Exceptions Outside of a Razor Component's Lifecycle](#handle-caught-exceptions-outside-of-a-razor-components-lifecycle)
    * [Detailed Errors](#detailed-errors)
      * [Circuit Errors](#circuit-errors) 
      * [Razor Component Server-side Rendering](#razor-component-server-side-rendering)
    * [Global Exception Handling](#global-exception-handling)
      * [ErrorBoundary](#errorboundary)
* [Components](#components)
  * [Components Overview](#components-overview)
  * [Markup](#markup)
  * [Asynchronous Methods](#asynchronous-methods)
  * 

# Overview
Blazor is a .NET frontend web framework that supports both server-side rendering and client interactivity in a single programming model

Blazor apps are based on components. A component in Blazor is an element of UI, such as a page, dialog, or data entry form. Components are .NET C# classes built into .NET assemblies. The component class is usually written in the form of a Razor markup page with a `.razor` file extension.

Blazor Web Apps provide a component-based architecture with server-side rendering and full client-side interactivity in a single solution, where you can switch between server-side and client-side rendering modes and even mix them in the same page.

### Full-stack web app
* **Server side static** <br>Blazor Web Apps can quickly deliver UI to the browser by statically rendering HTML content from the server in response to requests.

* **Server side interactive** <br>Blazor supports interactive server-side rendering (interactive SSR), where UI interactions are handled from the server over a real-time connection with the browser. Page content for interactive pages is prerendered, where content on the server is initially generated and sent to the client without enabling event handlers for rendered controls. The server outputs the HTML UI of the page as soon as possible in response to the initial request, which makes the app feel more responsive to users.

* **Client side webassembly** <br>Blazor Web Apps support interactivity with client-side rendering (CSR) that relies on a .NET runtime built with WebAssembly that you can download with your app. When running Blazor on WebAssembly, your .NET code can access the full functionality of the browser and interop with JavaScript.

* **Hybrid** <br>Blazor Hybrid enables using Razor components in a native client app with a blend of native and web technologies for web, mobile, and desktop platforms. Code runs natively in the .NET process and renders web UI to an embedded Web View control using a local interop channel. WebAssembly isn't used in Hybrid apps. Hybrid apps are built with .NET Multi-platform App UI (.NET MAUI), which is a cross-platform framework for creating native mobile and desktop apps with C# and XAML.

[*Reference - Microsoft ASP.NET Core Blazor : Overview*](https://learn.microsoft.com/en-us/aspnet/core/blazor)

### Supported Platforms
* Apple Safari
* Google Chrome
* Microsoft Edge
* Mozilla Firefox

[*Reference - Microsoft ASP.NET Core Blazor : Supported Platforms*](https://learn.microsoft.com/en-us/aspnet/core/blazor/supported-platforms)

### Hosting Models
##### Blazor Server hosting model
With the Blazor Server hosting model, components are executed on the server from within an **ASP.NET Core app**. UI updates, event handling, and JavaScript calls are handled over a **SignalR** connection using the **WebSockets** protocol. The state on the server associated with each connected client is called a **circuit**.

For the Blazor Server hosting model, **each browser screen requires a separate circuit and separate instances of server-managed component state**.
Blazor considers closing a browser tab or navigating to an external URL a graceful termination. In the event of a graceful termination, the circuit and associated resources are immediately released. A client may also disconnect non-gracefully, for instance due to a network interruption. Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.
On the client, the Blazor script establishes the SignalR connection with the server.

We recommend using the **Azure SignalR Service** for apps that adopt the Blazor Server hosting model. The service allows for scaling up a Blazor Server app to a large number of concurrent **SignalR** connections.

##### Blazor WebAssembly
The Blazor WebAssembly hosting model runs components client-side in the browser on a **WebAssembly-based .NET runtime**. Razor components, their dependencies, and the .NET runtime are downloaded to the browser. Components are executed directly on the browser UI thread. Unused code is stripped out of the app when it's published by the **Intermediate Language (IL) Trimmer**.
A Blazor WebAssembly app built as a **Progressive Web App (PWA)** uses modern browser APIs to enable many of the capabilities of a native client app, such as working offline, running in its own app window, launching from the host's operating system, receiving push notifications, and automatically updating in the background.

##### Blazor Hybrid
Blazor Hybrid apps can be built using different .NET native app frameworks, including **.NET MAUI**, **WPF**, and **Windows Forms**. Blazor provides `BlazorWebView` controls for adding Razor components to apps built with these frameworks.

[*Reference - Microsoft ASP.NET Core Blazor : Hosting Models*](https://learn.microsoft.com/en-us/aspnet/core/blazor/hosting-models)

# Fundamentals
### Static and interactive rendering concepts
Razor components are either statically rendered or interactively rendered.

Static or **static rendering** is a **server-side scenario** that means the component is rendered without the capacity for interplay between the user and .NET/C# code. JavaScript and HTML DOM events remain unaffected, but no user events on the client can be processed with .NET running on the server.

Interactive or **interactive rendering** means that the **component has the capacity to process .NET events via C# code**. The .NET events are either **processed on the server** by the ASP.NET Core runtime **or in the browser** on the client by the WebAssembly-based Blazor runtime.

[*Reference - Microsoft ASP.NET Core Blazor : Static and interactive rendering concepts*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals#static-and-interactive-rendering-concepts)

### Routing and navigation
If **prerendering** isn't disabled, the Blazor router (Router component, `<Router>` in `Routes.razor`) performs static routing to components during **static server-side rendering** (static SSR). This type of routing is called static routing.

When an **interactive render mode** is assigned to the Routes component, the Blazor router becomes ***interactive after static SSR*** with static routing on the server. This type of routing is called interactive routing.

> [!TIP]
> ***Static routers use endpoint routing and the HTTP request path to determine which component to render**. When the **router becomes interactive, it uses the document's URL (the URL in the browser's address bar) to determine which component to render**, and it can do so without performing an HTTP request to fetch new page content.*

***Interactive routing also prevents prerendering*** because new page content isn't requested from the server with a normal page request.

When a Razor component `.razor` with an `@page` directive is compiled, the generated component class is provided a `RouteAttribute` specifying the component's route template. Routing in Blazor is achieved by providing a route template to each accessible component in the app. 

The `Router` component enables routing to Razor components and is located in the app's Routes component, `Components/Routes.razor`. At runtime, the router searches for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.

At runtime, the `RouteView` component:
- Receives the `RouteData` from the Router along with any route parameters.
- Renders the specified component with its layout, including any further nested layouts.

Components support multiple route templates using multiple `@page` directives. 
```C#
@page "/blazor-route"
@page "/different-blazor-route"
```

##### Focus an Element on Navigation
The `FocusOnNavigate` component sets the UI focus to an element based on a CSS selector after navigating from one page to another.
```C#
<FocusOnNavigate RouteData="@routeData" Selector="h1" />
```

##### Route to Components from Multiple Assemblies
Use the Router component's `AdditionalAssemblies` parameter and the endpoint convention builder `AddAdditionalAssemblies` to discover routable components in additional assemblies. 

##### Interactive Routing
An interactive render mode can be assigned to the Routes component `Routes.razor`, that makes the Blazor router become interactive after static SSR and static routing on the server.
```
<Routes @rendermode="InteractiveServer" />
```

The `Router` component inherits interactive server-side rendering (interactive SSR) from the `Routes` component. **The router becomes interactive after static routing on the server**.

> [!TIP]
> Internal navigation for interactive routing does not involve requesting new page content from the server. Therefore, prerendering does not occur for internal page requests.

If the `Routes` component is defined in the server project, the `AdditionalAssemblies` parameter of the `Router` component should include the `.Client` project's assembly. This allows the router to work correctly when rendered interactively. Additional assemblies are scanned in addition to the assembly specified to AppAssembly.
```C#
<Router
    AppAssembly="..."
    AdditionalAssemblies="new[] { typeof(BlazorSample.Client._Imports).Assembly }">
    ...
</Router>
```

##### Route Parameters
Route parameter names are case insensitive. Optional parameters are supported.
```C#
@page "/route-parameter-2/{text?}"
```

A route constraint enforces type matching on a route segment to a component.
```C#
@page "/user/{Id:int}"
```

Catch-all route parameters, which capture paths across multiple folder boundaries, are supported. Catch-all route parameters are named to match the route segment name and must be a string type.
```C#
@page "/catch-all/{*pageRoute}"
```

Components can specify route parameters in the route template of the `@page` directive. The Blazor router uses route parameters to populate corresponding component parameters.

```C#
@page "/optional-parameter/{text?}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string? Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

##### NavigationManager
Use `NavigationManager` to manage URIs and navigation.

* **NavigationManager.NavigateTo** - if `forceLoad == true`, client-side routing is bypassed and the browser is forced to load the new page from the server

* **NavigationManager.LocationChanged** - event fired when the navigation path has changed. If `IsNavigationIntercepted == true`, then the Blazor intercepted the navigation from the browser, else `NavigationManager.NavigateTo` caused the navigation to occur.

* **NavigationManager.Refresh(bool forceLoad = false)** - refreshes the page

[*Reference - Microsoft ASP.NET Core Blazor : Routing and Navigation*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/routing)

### Dependency Injection
Register common services

If one or more common services are required client- and server-side, you can place the common service registrations in a method client-side and call the method to register the services in both projects.

##### Service Lifetime
**Scoped**
- Client-side doesn't currently have a concept of DI scopes. Scoped-registered services behave like Singleton services.
- Server-side development supports the Scoped lifetime across HTTP requests but not across SignalR connection/circuit messages among components that are loaded on the client. It recreates the services on each HTTP request.

**Singleton**
- All components requiring a Singleton service receive the same instance of the service.

**Transient**
- Whenever a component obtains an instance of a Transient service from the service container, it receives a new instance of the service.

Inject the services into the components using the `@inject` Razor directive or `[Inject]` attribute.
```C#
@inject IDataAccess DataRepository

[Inject]
protected IDataAccess DataRepository { get; set; } = default!;
```

##### Keyed Services
Blazor supports injecting keyed services using the `[Inject]` attribute. Keys allow for scoping of registration and consumption of services when using dependency injection.
```C#
[Inject(Key = "my-service")]
public IMyService MyService { get; set; }
```

> [!TIP]
> In ASP.NET Core apps, scoped services are typically scoped to the current request. After the request completes, any scoped or transient services are disposed by the DI system. Server-side, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected. Client-side, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.

##### OwningComponentBase
**OwningComponentBase** is an abstract type derived from `ComponentBase` that creates a DI scope corresponding to the lifetime of the component. Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.

**OwningComponentBase** is an abstract, disposable child of the `ComponentBase` type with a protected `ScopedServices` property of type `IServiceProvider`. The provider can be used to resolve services that are scoped to the lifetime of the component.
```C#
@code {
    private ITimeTravel TimeTravel2 { get; set; } = default!;

    protected override void OnInitialized()
    {
        TimeTravel2 = ScopedServices.GetRequiredService<ITimeTravel>();
    }
}
```

> [!TIP]
> Regular scoped objects injected into the component using `@inject` or the `[Inject]` attribute are tied to the user's circuit, which remains intact and isn't disposed until the underlying circuit is deconstructed. Scoped objects created using `ScopedServices.GetRequiredService<ITimeTravel>();` receives a new ITimeTravel service instance each time the component is initialized.

[*Reference - Microsoft ASP.NET Core Blazor : Dependency Injection*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/dependency-injection)

### Startup
##### Startup Process
The Blazor startup process is automatic and asynchronous via the Blazor script `blazor.*.js`, where the * placeholder is:
- web for a Blazor Web App
- server for a Blazor Server app
- webassembly for a Blazor WebAssembly app

##### Load Client-side Boot Resources
When an app loads in the browser, the app downloads boot resources from the server:
- JavaScript code to bootstrap the app
- .NET runtime and assemblies
- Locale specific data

##### Control Headers in Code
**Server-side and Prerendered Client-side Scenarios**
<br>
Use ASP.NET Core Middleware to control the headers collection.
```C#
app.Use(async (context, next) =>
{
    context.Response.Headers.Add("Content-Security-Policy", "{POLICY STRING}");
    await next();
});
```

**Client-side Development Without Prerendering**
<br>
Pass `StaticFileOptions` to `MapFallbackToFile` that specifies response headers at the `OnPrepareResponse` stage.
```C#
var staticFileOptions = new StaticFileOptions
{
    OnPrepareResponse = context =>
    {
        context.Context.Response.Headers.Add("Content-Security-Policy", 
            "{POLICY STRING}");
    }
};

...

app.MapFallbackToFile("index.html", staticFileOptions);
```
[*Reference - Microsoft ASP.NET Core Blazor : Startup*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/startup)

### Logging
Logging configuration can be loaded from app settings files. For more information, see ASP.NET Core Blazor configuration.

At default log levels and without configuring additional logging providers:
- **On the server**, logging only occurs to the server-side .NET console in the Development environment at the LogLevel.Information level or higher. For general ASP.NET Core logging guidance, see [Logging in .NET Core and ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/logging).
- **On the client**, logging only occurs to the client-side browser developer tools console at the LogLevel.Information level or higher.

[*Reference - Microsoft ASP.NET Core Blazor : Logging*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/logging).

### Exceptions
##### Handle Caught Exceptions Outside of a Razor Component's Lifecycle
Use `ComponentBase.DispatchExceptionAsync` in a Razor component to process exceptions thrown outside of the component's lifecycle call stack. This permits the component's code to treat exceptions as though they're lifecycle method exceptions. Thereafter, Blazor's error handling mechanisms, such as error boundaries, can process the exceptions.
<br>
`ComponentBase.DispatchExceptionAsync` is used in Razor component files that inherit from `ComponentBase`.
```C#
try
{
    ...
}
catch (Exception ex)
{
    await DispatchExceptionAsync(ex);
}
```
A common scenario is if a component wants to start an asynchronous operation but doesn't await a Task. If the operation fails, you may still want the component to treat the failure as a component lifecycle exception.

##### Detailed Errors
###### Circuit Errors
This applies to Blazor Web Apps operating over a circuit.
<br>
For development purposes, sensitive circuit error information can be made available to the client by enabling detailed errors.
<br>
Set the DetailedErrors configuration key to true in the app's Development environment settings file `appsettings.Development.json`. Additionally, set SignalR server-side logging (Microsoft.AspNetCore.SignalR) to Debug or Trace for detailed SignalR logging.
```C#
{
  "DetailedErrors": true,
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information",
      "Microsoft.AspNetCore.SignalR": "Debug"
    }
  }
}
```

###### Razor Component Server-side Rendering
Use the `RazorComponentsServiceOptions.DetailedErrors` option to control producing detailed information on errors for Razor component server-side rendering. The default value is `false`.
```C#
builder.Services.AddRazorComponents(options => options.DetailedErrors = true);
```

> [!WARNING]
> Only enable detailed errors in the Development environment.

##### Global Exception Handling

###### ErrorBoundary
To define an error boundary, use the `ErrorBoundary` component to wrap existing content. The app continues to function normally, but the error boundary handles unhandled exceptions.
```C#
<article class="content px-4">
    <ErrorBoundary>
        <ChildContent>
            @Body
        </ChildContent>
        <ErrorContent>
            <p class="alert alert-danger" role="alert">
                Oh, dear! Oh, my! - George Takei
            </p>
        </ErrorContent>
    </ErrorBoundary>
</article>
```
> [!TIP]
> To implement an error boundary in a global fashion, add the boundary around the body content of the app's main layout.
>
> In Blazor Web Apps with the error boundary only applied to a static `MainLayout` component, the boundary is only active during the static server-side rendering (static SSR) phase. The boundary doesn't activate just because a component further down the component hierarchy is interactive. To enable interactivity broadly for the `MainLayout` component and the rest of the components further down the component hierarchy, enable interactive server-side rendering (interactive SSR) at the top of the `Routes` component `Components/Routes.razor`.
>
>If you prefer not to enable server interactivity across the entire app from the `Routes` component, place the error boundary further down the component hierarchy.

[*Reference - Microsoft ASP.NET Core Blazor : Handle Errors*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/handle-errors).

# Components
### Components Overview
Blazor apps are built using Razor components with the `.razor` file extension using a combination of C# and HTML markup, that render into an in-memory representation of the browser's `Document Object Model (DOM)` called a render tree, which is used to update the UI.

Components are ordinary C# classes and `ComponentBase` is the base class for components, defining properties and methods for basic functionality, like processing a set of built-in component lifecycle events.

Components are generated as C# partial classes and can be created as:
- A single file contains C# code defined in one or more @code blocks, HTML markup, and Razor markup.
- HTML and Razor markup are placed in a Razor file `.razor`. C# code is placed in a code-behind file defined as a partial class `.cs`.

Razor components use directives to hange the way component markup is parsed or functions. 
Razor component directive order:
```C#
@page
@rendermode (.NET 8 or later)
@using
 - System namespaces
 - Microsoft namespaces
 - App namespaces
 - Other directives
```

`@using` directives in the `_Imports.razor` file are only applied to Razor files `.razor`, not C# files `.cs`.

A component's name must start with an uppercase character e.g. `ProductDetail.razor`.

Component file paths for routable components match their URLs in kebab case. For example, a `ProductDetail.razor` component with a route template of `@page "/product-detail"` is requested in a browser at the relative URL `/product-detail`.

### Markup
When an app is compiled, the HTML markup and C# rendering logic are converted into a component class. Members of the component class are defined in one or more `@code` blocks.

> [!NOTE]
> The Blazor framework processes a component internally as a [render tree](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work#render), which is the combination of a component's DOM and [Cascading Style Sheet Object Model (CSSOM)](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model). After the component is initially rendered, the component's render tree is regenerated in response to events. Blazor compares the new render tree against the previous render tree and applies any modifications to the browser's DOM for display.

### Asynchronous Methods
> [!WARNING]
> The Blazor framework doesn't track void-returning asynchronous methods `async`. As a result, exceptions aren't caught if void is returned. Always return a `Task` from asynchronous methods.

Unlike in Razor pages (.cshtml), Blazor can't perform asynchronous work in a Razor expression while rendering a component. This is because Blazor is designed for rendering interactive UIs.



[*Reference - Microsoft ASP.NET Core Blazor : Components*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components)
<br>
[*Reference - How Browsers Work*](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)
<br>
[*Reference - Cascading Style Sheet Object Model (CSSOM)*](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)

































