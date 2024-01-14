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

The Router component enables routing to Razor components and is located in the app's Routes component, `Components/Routes.razor`.

When a Razor component, `.razor`, with an `@page` directive is compiled, the generated component class is provided a `RouteAttribute` specifying the component's route template.

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
An interactive render mode can be assigned to the Routes component (`Routes.razor`) that makes the Blazor router become interactive after static SSR and static routing on the server.
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

##### NavigationManager
Use `NavigationManager` to manage URIs and navigation.

* **NavigationManager.NavigateTo** - if `forceLoad == true`, client-side routing is bypassed and the browser is forced to load the new page from the server

* **NavigationManager.LocationChanged** - event fired when the navigation path has changed. If `IsNavigationIntercepted == true`, then the Blazor intercepted the navigation from the browser, else `NavigationManager.NavigateTo` caused the navigation to occur.

* **NavigationManager.Refresh(bool forceLoad = false)** - refreshes the page

[*Reference - Microsoft ASP.NET Core Blazor : Routing and Navigation*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/routing)


https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/dependency-injection?view=aspnetcore-8.0
Register common services
If one or more common services are required client- and server-side, you can place the common service registrations in a method client-side and call the method to register the services in both projects.

Service lifetime
Scoped
- Client-side doesn't currently have a concept of DI scopes. Scoped-registered services behave like Singleton services.
- Server-side development supports the Scoped lifetime across HTTP requests but not across SignalR connection/circuit messages among components that are loaded on the client. It recreates the services on each HTTP request.

Singleton
- All components requiring a Singleton service receive the same instance of the service.

Transient
- Whenever a component obtains an instance of a Transient service from the service container, it receives a new instance of the service.

inject the services into the components using the @inject Razor directive or [Inject] attribute.
@inject IDataAccess DataRepository

[Inject]
protected IDataAccess DataRepository { get; set; } = default!;

Inject keyed services into components
Blazor supports injecting keyed services using the [Inject] attribute. Keys allow for scoping of registration and consumption of services when using dependency injection.
[Inject(Key = "my-service")]
public IMyService MyService { get; set; }

In ASP.NET Core apps, scoped services are typically scoped to the current request. After the request completes, any scoped or transient services are disposed by the DI system. Server-side, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected. Client-side, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.

OwningComponentBase
OwningComponentBase is an abstract type derived from ComponentBase that creates a DI scope corresponding to the lifetime of the component. Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.
OwningComponentBase is an abstract, disposable child of the ComponentBase type with a protected ScopedServices property of type IServiceProvider. The provider can be used to resolve services that are scoped to the lifetime of the component.
@code {
    private ITimeTravel TimeTravel2 { get; set; } = default!;

    protected override void OnInitialized()
    {
        TimeTravel2 = ScopedServices.GetRequiredService<ITimeTravel>();
    }
}

NOTE: regular scoped objects injected into the component using @inject or the [Inject] attribute are tied to the user's circuit, which remains intact and isn't disposed until the underlying circuit is deconstructed. Scoped objects created using ScopedServices.GetRequiredService<ITimeTravel>(); receives a new ITimeTravel service instance each time the component is initialized.

https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/startup?view=aspnetcore-8.0
Startup










































