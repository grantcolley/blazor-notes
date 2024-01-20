# blazor-notes
##### ASP.NET Core Blazor .NET 8.0

> [!IMPORTANT]
> This is a collection of notes about **ASP.NET Core Blazor** taken while reading other resources\*. The purpose of this `readme` is simply to consolidate those notes I find important or useful to me in some way, and condense them into a single page for my own personal reference.
> 
> In many cases, the notes may be straight copy and paste. In others, I may shorten or reword it, as per my understanding of the original text. In all cases, the original content is not my own and I provide links to the original source\*.
>
> \* *the content is mostly from [**Microsoft's Blazor documentation**](https://learn.microsoft.com/en-us/aspnet/core/blazor), with some pointers to [**Microsoft's ASP.NET Core documentation**](https://learn.microsoft.com/en-us/aspnet/core/fundamentals) and [**Mozilla MDN Web Docs**](https://developer.mozilla.org/en-US/docs/Web) for additional reading about broader web development concepts.*

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
    * [Enhanced Navigation and Form Handling](#enhanced-navigation-and-form-handling)
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
    * [Render Fragments](#render-fragments)
    * [Capture References to Components](#capture-references-to-components)
    * [Static Assets](#static-assets)
    * [Root Component](#root-component)
  * [Render Modes](#render-modes)
    * [Enabling Interactive Render Modes](#enabling-interactive-render-modes)
    * [Apply a Render Mode to a Component](#apply-a-render-mode-to-a-component)
      * [Apply a Render Mode to a Component Instance](#apply-a-render-mode-to-a-component-instance)
      * [Apply a Render Mode to a Component Definition](#apply-a-render-mode-to-a-component-definition)
    * [Apply a Render Mode to the Entire App](#apply-a-render-mode-to-the-entire-app)
    * [Prerendering](#prerendering)
    * [Static Server-side Rendering (Static SSR)](#static-server-side-rendering-static-ssr)
    * [Interactive Server-side Rendering (Interactive SSR)](#interactive-server-side-rendering-interactive-ssr)
    * [Client-side Rendering (CSR)](#client-side-rendering-csr)
    * [Automatic (Auto) Rendering](#automatic-auto-rendering)
    * [Client-side Services fail to Resolve during Prerendering](#client-side-services-fail-to-resolve-during-prerendering)
    * [Render Mode Propagation](#render-mode-propagation)
    * [Render Mode Inheritance](#render-mode-inheritance)
    * [Child Components with Different Render Modes](#child-components-with-different-render-modes)
    * [Interactive Component Parameters must be Serializable](#interactive-component-parameters-must-be-serializable)
    * [Child Component must have same Render Mode as it's Parent](#child-component-must-have-same-render-mode-as-its-parent)
    * [Closure of Circuits when there are no remaining Interactive Server Components](#closure-of-circuits-when-there-are-no-remaining-interactive-server-components)
  * [Prerender ASP.NET Core Razor Components](#prerender-aspnet-core-razor-components)
    * [Persist Prerendered State](#persist-prerendered-state)
  * [Generic Type Support](#generic-type-support)
    * [Generic Type Parameter Support](#generic-type-parameter-support)  
    * [Cascaded Generic Type Support](#cascaded-generic-type-support)
  * [Synchronization Context](#synchronization-context)
    * [Avoid Thread-blocking Calls](#avoid-thread-blocking-calls)
    * [Invoke Component Methods Externally to Update State](#invoke-component-methods-externally-to-update-state)
  * [Preserve Relationships with @key](#preserve-relationships-with-key)
  * [Overwriting Parameters](#overwriting-parameters)
  * [Attribute Splatting](#attribute-splatting)
  * [Layouts](#layouts)
    * [Apply a Layout to a Component](#apply-a-layout-to-a-component)
    * [Apply a Layout to a Folder of Components](#apply-a-layout-to-a-folder-of-components)
    * [Apply a Default Layout to an App](#apply-a-default-layout-to-an-app)
  * [Sections](#sections)
  * [Control \<head\> Content](#control-head-content)

# Overview
Blazor is a .NET frontend web framework that supports both server-side rendering and client interactivity in a single programming model

Blazor apps are based on components. A component in Blazor is an element of UI, such as a page, dialog, or data entry form. Components are .NET C# classes built into .NET assemblies. The component class is usually written in the form of a Razor markup page with a `.razor` file extension.

Blazor Web Apps provide a component-based architecture with server-side rendering and full client-side interactivity in a single solution, where you can switch between server-side and client-side rendering modes and even mix them in the same page.

## Full-stack web app
* **Server side static** <br>Blazor Web Apps can quickly deliver UI to the browser by statically rendering HTML content from the server in response to requests.

* **Server side interactive** <br>Blazor supports interactive server-side rendering (interactive SSR), where UI interactions are handled from the server over a real-time connection with the browser. Page content for interactive pages is prerendered, where content on the server is initially generated and sent to the client without enabling event handlers for rendered controls. The server outputs the HTML UI of the page as soon as possible in response to the initial request, which makes the app feel more responsive to users.

* **Client side webassembly** <br>Blazor Web Apps support interactivity with client-side rendering (CSR) that relies on a .NET runtime built with WebAssembly that you can download with your app. When running Blazor on WebAssembly, your .NET code can access the full functionality of the browser and interop with JavaScript.

* **Hybrid** <br>Blazor Hybrid enables using Razor components in a native client app with a blend of native and web technologies for web, mobile, and desktop platforms. Code runs natively in the .NET process and renders web UI to an embedded Web View control using a local interop channel. WebAssembly isn't used in Hybrid apps. Hybrid apps are built with .NET Multi-platform App UI (.NET MAUI), which is a cross-platform framework for creating native mobile and desktop apps with C# and XAML.

[*Source - Microsoft ASP.NET Core Blazor : Overview*](https://learn.microsoft.com/en-us/aspnet/core/blazor)

## Supported Platforms
* Apple Safari
* Google Chrome
* Microsoft Edge
* Mozilla Firefox

[*Source - Microsoft ASP.NET Core Blazor : Supported Platforms*](https://learn.microsoft.com/en-us/aspnet/core/blazor/supported-platforms)

## Hosting Models
### Blazor Server hosting model
With the Blazor Server hosting model, components are executed on the server from within an **ASP.NET Core app**. UI updates, event handling, and JavaScript calls are handled over a **SignalR** connection using the **WebSockets** protocol. The state on the server associated with each connected client is called a **circuit**.

For the Blazor Server hosting model, **each browser screen requires a separate circuit and separate instances of server-managed component state**.
Blazor considers closing a browser tab or navigating to an external URL a graceful termination. In the event of a graceful termination, the circuit and associated resources are immediately released. A client may also disconnect non-gracefully, for instance due to a network interruption. Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.
On the client, the Blazor script establishes the SignalR connection with the server.

We recommend using the **Azure SignalR Service** for apps that adopt the Blazor Server hosting model. The service allows for scaling up a Blazor Server app to a large number of concurrent **SignalR** connections.

### Blazor WebAssembly
The Blazor WebAssembly hosting model runs components client-side in the browser on a **WebAssembly-based .NET runtime**. Razor components, their dependencies, and the .NET runtime are downloaded to the browser. Components are executed directly on the browser UI thread. Unused code is stripped out of the app when it's published by the **Intermediate Language (IL) Trimmer**.
A Blazor WebAssembly app built as a **Progressive Web App (PWA)** uses modern browser APIs to enable many of the capabilities of a native client app, such as working offline, running in its own app window, launching from the host's operating system, receiving push notifications, and automatically updating in the background.

### Blazor Hybrid
Blazor Hybrid apps can be built using different .NET native app frameworks, including **.NET MAUI**, **WPF**, and **Windows Forms**. Blazor provides `BlazorWebView` controls for adding Razor components to apps built with these frameworks.

[*Source - Microsoft ASP.NET Core Blazor : Hosting Models*](https://learn.microsoft.com/en-us/aspnet/core/blazor/hosting-models)

# Fundamentals
### Static and interactive rendering concepts
Razor components are either statically rendered or interactively rendered.

Static or **static rendering** is a **server-side scenario** that means the component is rendered without the capacity for interplay between the user and .NET/C# code. JavaScript and HTML DOM events remain unaffected, but no user events on the client can be processed with .NET running on the server.

Interactive or **interactive rendering** means that the **component has the capacity to process .NET events via C# code**. The .NET events are either **processed on the server** by the ASP.NET Core runtime **or in the browser** on the client by the WebAssembly-based Blazor runtime.

[*Source - Microsoft ASP.NET Core Blazor : Static and interactive rendering concepts*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals#static-and-interactive-rendering-concepts)

## Routing and navigation
If **prerendering** isn't disabled, the Blazor router (Router component, `<Router>` in `Routes.razor`) performs static routing to components during **static server-side rendering** (static SSR). This type of routing is called static routing.

When an **interactive render mode** is assigned to the Routes component, the Blazor router becomes ***interactive after static SSR*** with static routing on the server. This type of routing is called interactive routing.

> [!IMPORTANT]
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

### Focus an Element on Navigation
The `FocusOnNavigate` component sets the UI focus to an element based on a CSS selector after navigating from one page to another.
```C#
<FocusOnNavigate RouteData="@routeData" Selector="h1" />
```

### Route to Components from Multiple Assemblies
Use the Router component's `AdditionalAssemblies` parameter and the endpoint convention builder `AddAdditionalAssemblies` to discover routable components in additional assemblies. 

### Interactive Routing
An interactive render mode can be assigned to the Routes component `Routes.razor`, that makes the Blazor router become interactive after static SSR and static routing on the server.
```
<Routes @rendermode="InteractiveServer" />
```

The `Router` component inherits interactive server-side rendering (interactive SSR) from the `Routes` component. **The router becomes interactive after static routing on the server**.

> [!IMPORTANT]
> Internal navigation for interactive routing does not involve requesting new page content from the server. Therefore, prerendering does not occur for internal page requests.

If the `Routes` component is defined in the server project, the `AdditionalAssemblies` parameter of the `Router` component should include the `.Client` project's assembly. This allows the router to work correctly when rendered interactively. Additional assemblies are scanned in addition to the assembly specified to AppAssembly.
```C#
<Router
    AppAssembly="..."
    AdditionalAssemblies="new[] { typeof(BlazorSample.Client._Imports).Assembly }">
    ...
</Router>
```

### Route Parameters
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

### NavigationManager
Use `NavigationManager` to manage URIs and navigation.

* **NavigationManager.NavigateTo** - if `forceLoad == true`, client-side routing is bypassed and the browser is forced to load the new page from the server

* **NavigationManager.LocationChanged** - event fired when the navigation path has changed. If `IsNavigationIntercepted == true`, then the Blazor intercepted the navigation from the browser, else `NavigationManager.NavigateTo` caused the navigation to occur.

* **NavigationManager.Refresh(bool forceLoad = false)** - refreshes the page

### Enhanced Navigation and Form Handling

Enhanced navigation is enabled by default.

Blazor Web Apps are capable of two types of routing for page navigation and form handling requests:
- **Normal navigation (cross-document navigation)**: a full-page reload is triggered for the request URL.
- **Enhanced navigation (same-document navigation)**: Blazor intercepts the request and performs a fetch request instead. Blazor then patches the response content into the page's DOM. Blazor's enhanced navigation and form handling avoid the need for a full-page reload and preserves more of the page state, so pages load faster, usually without losing the user's scroll position on the page.

**Enhanced navigation** is available when:
- The Blazor Web App script `blazor.web.js` is used, not the Blazor Server script `blazor.server.js` or Blazor WebAssembly script `blazor.webassembly.js`.
- The feature isn't explicitly disabled.
- The destination URL is within the internal base URI space (the app's base path).

If server-side routing and enhanced navigation are enabled, location changing handlers are only invoked for programmatic navigation initiated from an interactive runtime. In future releases, additional types of navigation, such as link clicks, may also invoke location changing handlers.

When an enhanced navigation occurs, `LocationChanged` event handlers registered with Interactive Server and WebAssembly runtimes are typically invoked. There are cases when location changing handlers might not intercept an enhanced navigation. For example, the user might switch to another page before an interactive runtime becomes available.

When calling `NavigateTo`:
- If **forceLoad is false**, which is the default: And enhanced navigation is available at the current URL, Blazor's enhanced navigation is activated. Otherwise, Blazor performs a full-page reload for the requested URL.
- If **forceLoad is true**: Blazor performs a full-page reload for the requested URL, whether enhanced navigation is available or not.

You can refresh the current page by calling `NavigationManager.Refresh(bool forceLoad = false)`, which always performs an enhanced navigation, if available. If enhanced navigation isn't available, Blazor performs a full-page reload.

[*Source - Microsoft ASP.NET Core Blazor : Routing and Navigation*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/routing)

## Dependency Injection
Register common services

If one or more common services are required client- and server-side, you can place the common service registrations in a method client-side and call the method to register the services in both projects.

### Service Lifetime
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

### Keyed Services
Blazor supports injecting keyed services using the `[Inject]` attribute. Keys allow for scoping of registration and consumption of services when using dependency injection.
```C#
[Inject(Key = "my-service")]
public IMyService MyService { get; set; }
```

> [!TIP]
> In ASP.NET Core apps, scoped services are typically scoped to the current request. After the request completes, any scoped or transient services are disposed by the DI system. Server-side, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected. Client-side, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.

### OwningComponentBase
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

[*Source - Microsoft ASP.NET Core Blazor : Dependency Injection*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/dependency-injection)

## Startup
### Startup Process
The Blazor startup process is automatic and asynchronous via the Blazor script `blazor.*.js`, where the * placeholder is:
- web for a Blazor Web App
- server for a Blazor Server app
- webassembly for a Blazor WebAssembly app

### Load Client-side Boot Resources
When an app loads in the browser, the app downloads boot resources from the server:
- JavaScript code to bootstrap the app
- .NET runtime and assemblies
- Locale specific data

### Control Headers in Code
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
[*Source - Microsoft ASP.NET Core Blazor : Startup*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/startup)

## Logging
Logging configuration can be loaded from app settings files. For more information, see ASP.NET Core Blazor configuration.

At default log levels and without configuring additional logging providers:
- **On the server**, logging only occurs to the server-side .NET console in the Development environment at the LogLevel.Information level or higher. For general ASP.NET Core logging guidance, see [Logging in .NET Core and ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/logging).
- **On the client**, logging only occurs to the client-side browser developer tools console at the LogLevel.Information level or higher.

[*Source - Microsoft ASP.NET Core Blazor : Logging*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/logging).

## Exceptions
### Handle Caught Exceptions Outside of a Razor Component's Lifecycle
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

### Detailed Errors
#### Circuit Errors
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

#### Razor Component Server-side Rendering
Use the `RazorComponentsServiceOptions.DetailedErrors` option to control producing detailed information on errors for Razor component server-side rendering. The default value is `false`.
```C#
builder.Services.AddRazorComponents(options => options.DetailedErrors = true);
```

> [!WARNING]
> Only enable detailed errors in the Development environment.

### Global Exception Handling

#### ErrorBoundary
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

[*Source - Microsoft ASP.NET Core Blazor : Handle Errors*](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/handle-errors).

# Components
## Components Overview
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

Unlike in Razor pages (.cshtml), Blazor can't perform asynchronous work in a Razor expression while rendering a component. This is because Blazor is designed for rendering interactive UIs. In an interactive UI, the screen must always display something, so it doesn't make sense to block the rendering flow. Instead, asynchronous work is performed during one of the asynchronous lifecycle events. After each asynchronous lifecycle event, the component may render again.

### Render Fragments
Components can set the content of another component using `RenderFragment`.

`RenderFragmentChild.razor`:
```C#
<div class="card-body">
    @ChildContent
</div>

@code {
    [Parameter]
    public RenderFragment? ChildContent { get; set; }
}
```
The following component provides content for rendering the `RenderFragmentChild.razor` by placing the content inside the child component's opening and closing tags.
```C#
@page "/render-fragments"

<h1>Render Fragments Example</h1>

<RenderFragmentChild>
    Content of the child component is supplied
    by the parent component.
</RenderFragmentChild>
```

### Capture References to Components
To capture a component reference:
- Add an `@ref` attribute to the child component.
- Define a field with the same type as the child component.
When the component is rendered, the field is populated with the component instance. You can then invoke .NET methods on the instance.

`ReferenceChild.razor`:
```C#
@if (value > 0)
{
    <p>
        <code>value</code>: @value
    </p>
}

@code {
    private int value;

    public void ChildMethod(int value)
    {
        this.value = value;
        StateHasChanged();
    }
}
```
A component reference is only populated after the component is rendered and its output includes `ReferenceChild`'s element. Until the component is rendered, there's nothing to reference.
<br>
To manipulate component references after the component has finished rendering, use the OnAfterRender or OnAfterRenderAsync methods.

`ReferenceParent1.razor`:
```C#
@page "/reference-parent-1"

<button @onclick="@(() => childComponent?.ChildMethod(5))">
    Call <code>ReferenceChild.ChildMethod</code> with an argument of 5
</button>

<ReferenceChild @ref="childComponent" />

@code {
    private ReferenceChild? childComponent;
}
```

### Static Assets
Static assets are located in the project's [web root (wwwroot)](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/#web-root) folder or folders under the `wwwroot` folder.

Use a base-relative path (/) to refer to the web root for a static asset.
```C#
<img alt="Company logo" src="/images/logo.png" />
```

### Root Component
A root Razor component (root component) is the first component loaded of any component hierarchy created by the app.

In an app created from the Blazor Web App project template, the App component (App.razor) is specified as the default root component in the `Program.cs` file.

**Server-side**
```C#
app.MapRazorComponents<App>();
```

**WebAssembly**
```C#
builder.RootComponents.Add<App>("#app");
```

[*Source - Microsoft ASP.NET Core Blazor : Components*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components)
<br>
[*Source - How Browsers Work*](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)
<br>
[*Source - Cascading Style Sheet Object Model (CSSOM)*](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)
<br>
[*Source - ASP.NET Core fundamentals overview - Web root*](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/#web-root)

## Render Modes
Every component in a Blazor Web App adopts a render mode to determine the hosting model that it uses, where it's rendered, and whether or not it's interactive.

Render mode is applied using a `@rendermode` directive on the component instance or on the component definition

- **Static Server** - Static server-side rendering (static SSR)	Server
- **Interactive Server** -	Interactive server-side rendering (interactive SSR) using Blazor Server	Server
- **Interactive WebAssembly** -	Client-side rendering (CSR) using Blazor WebAssembly
- **Interactive Auto** -	Interactive SSR using Blazor Server initially and then CSR on subsequent visits after the Blazor bundle is downloaded

> [!IMPORTANT]
> Prerendering is enabled by default for interactive components.

### Enabling Interactive Render Modes
A Blazor Web App must be configured to support interactive render modes in it's `Program.cs`.

**Component builder extensions** adds services to support rendering Interactive Server `AddInteractiveServerComponents` or Interactive WebAssembly components `AddInteractiveWebAssemblyComponents`.

`MapRazorComponents` discovers available components and specifies the root component for the app (the first component loaded), which by default is the App component (App.razor).

```C#
// adds services to support rendering Interactive Server components.
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// configures interactive server-side rendering (interactive SSR) for the app.
app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();
```

```C#
// adds services to support rendering Interactive WebAssembly components.
builder.Services.AddRazorComponents()
    .AddInteractiveWebAssemblyComponents();

// configures the Interactive WebAssembly render mode for the app.
app.MapRazorComponents<App>()
    .AddInteractiveWebAssemblyRenderMode();
```

> [!NOTE]
> Individual components are still required to declare their render mode.

### Apply a Render Mode to a Component

#### Apply a Render Mode to a Component Instance
```C#
<Dialog @rendermode="InteractiveServer" />
```

#### Apply a Render Mode to a Component Definition
```C#
@page "..."
@rendermode InteractiveServer
```

Applying a render mode to a component definition is commonly used when applying a render mode to a specific page.

Routable pages by default use the same render mode as the Router component that rendered the page.

> [!TIP]
> Component authors should avoid coupling a component's implementation to a specific render mode. Instead, component authors should typically design components to support any render mode or hosting model. A component's implementation should avoid assumptions on where it's running (server or client)

### Apply a Render Mode to the Entire App
To set the render mode for the entire app, indicate the render mode at the highest-level interactive component in the app's component hierarchy that isn't a root component. This is typically specified where the Routes component is used in the App component `Components/App.razor`:
```C#
<Routes @rendermode="InteractiveServer" />
```

### Prerendering
Prerendering is the process of initially rendering page content on the server without enabling event handlers for rendered controls.

> [!NOTE]
> Prerendering is enabled by default for interactive components.

To disable prerendering for a component instance, pass the prerender flag with a value of false to the render mode:
</br>
```C#
<... @rendermode="new InteractiveServerRenderMode(prerender: false)" />
```
To disable prerendering in a component definition:
</br>
```C#
@rendermode @(new InteractiveServerRenderMode(prerender: false))
```

### Static Server-side Rendering (Static SSR)
By default, components use the static server-side rendering (static SSR). The component renders to the response stream and interactivity isn't enabled.

##### Interactive Server-side Rendering (Interactive SSR)
Interactive server-side rendering (interactive SSR) renders the component interactively from the server using Blazor Server. User interactions are handled over a real-time connection with the browser. The circuit connection is established when the Server component is rendered.

Interactive SSR components become interactive after the SignalR circuit is established.

```C#
@rendermode InteractiveServer
```

### Client-side Rendering (CSR)
Client-side rendering (CSR) renders the component interactively on the client using Blazor WebAssembly. The .NET runtime and app bundle are downloaded and cached when the WebAssembly component is initially rendered. 

CSR components become interactive after the Blazor app bundle is downloaded and the .NET runtime is active on the client.

Components using CSR must be built from a separate client project that sets up the Blazor WebAssembly host.
```C#
@rendermode InteractiveWebAssembly
```
> [!TIP]
> Components using CSR must be built from a separate client project that sets up the Blazor WebAssembly host.

### Automatic (Auto) Rendering
The component is initially rendered with interactive server-side rendering (interactive SSR) using the Blazor Server hosting model. The .NET runtime and app bundle are downloaded to the client in the background and cached so that they can be used on future visits.

The Auto render mode never dynamically changes the render mode of a component already on the page. The Auto render mode makes an initial decision about which type of interactivity to use for a component, then the component keeps that type of interactivity for as long as it's on the page. One factor in this initial decision is considering whether components already exist on the page with WebAssembly/Server interactivity. Auto mode prefers to select a render mode that matches the render mode of existing interactive components. The reason that the Auto mode prefers to use an existing interactivity mode is to avoid introducing a new interactive runtime that doesn't share state with the existing runtime.
```C#
@rendermode InteractiveAuto
```
> [!TIP]
> Components using the Auto render mode must be built from a separate client project that sets up the Blazor WebAssembly host.

### Client-side Services fail to Resolve during Prerendering
Assuming that prerendering isn't disabled for a component or for the app, a component in the .Client project is prerendered on the server. Because the server doesn't have access to registered client-side Blazor services, it isn't possible to inject these services into a component without receiving an error that the service can't be found during prerendering.

The recomended approach is to register the services in the main project, which makes them available during prerendering. 

### Render Mode Propagation
Render modes propagate down the component hierarchy.

**Rules for applying render modes:**
- The default render mode is Static.
- The Interactive Server (InteractiveServer), Interactive WebAssembly (InteractiveWebAssembly), and Interactive Auto (InteractiveAuto) render modes can be used from a component, including using different render modes for sibling components.
- You can't switch to a different interactive render mode in a child component. For example, a Server component can't be a child of a WebAssembly component.
- Parameters passed to an interactive child component from a Static parent must be JSON serializable. This means that you can't pass render fragments or child content from a Static parent component to an interactive child component.

### Render Mode Inheritance
Child components inherit it's parent's render mode.

In the following example the `SharedMessage` component inherits it's paren't `@rendermode InteractiveServer`.
```C#
@page "/render-mode-6"
@rendermode InteractiveServer

<SharedMessage />
```

### Child Components with Different Render Modes
In the following example, both `SharedMessage` components are prerendered (by default).
- The first `SharedMessage` component with interactive server-side rendering (interactive SSR) is interactive after the **SignalR** circuit is established.
- The second `SharedMessage` component with client-side rendering (CSR) is interactive after the Blazor app bundle is downloaded and the .NET runtime is active on the client.

```C#
@page "/render-mode-7"

<SharedMessage @rendermode="InteractiveServer" />
<SharedMessage @rendermode="InteractiveWebAssembly" />
```

### Interactive Component Parameters must be Serializable
Parameters for interactive components must be serializable. Non-serializable component parameters, such as child content or a render fragment, are not supported, and will result in a `System.InvalidOperationException`.

### Child Component must have same Render Mode as it's Parent
Applying a different interactive render mode to a child component than its parent's render mode will result in a runtime error.

### Closure of Circuits when there are no remaining Interactive Server Components
Interactive Server components handle web UI events using a real-time connection with the browser called a circuit. A circuit and its associated state are created when a root Interactive Server component is rendered. The circuit is closed when there are no remaining Interactive Server components on the page, which frees up server resources.

[*Source - Microsoft ASP.NET Core Blazor : Render Modes*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/render-modes)

## Prerender ASP.NET Core Razor Components
Prerendering is the process of initially rendering page content on the server without enabling event handlers for rendered controls. The server outputs the HTML UI of the page as soon as possible in response to the initial request, which makes the app feel more responsive to users.

### Persist Prerendered State
State used during prerendering is lost and must be recreated when the app is fully loaded.

Component may set initial state during prerendering in `OnInitialized` lifecycle method. After the `SignalR` connection to the client is established, the component rerenders, and the initial state is replaced when `OnInitialized` executes a second time. If any state is created asynchronously, the UI may flicker as the prerendered UI is replaced when the component is rerendered.

State created during prerendering can be persisted to avoid having to recreate it when `OnInitialized` executes a second time.

> [!NOTE]
> If the app adopts [interactive (enhanced) routing](#enhanced-navigation-and-form-handling) and the page is reached via an internal navigation, prerendering doesn't occur.

The `PersistentComponentState` service can be used to persist state created during the prerendering stage. `PersistentComponentState.RegisterOnPersisting` registers a callback to persist the component state before the app is paused. The state is retrieved when the app resumes.

```C#
@implements IDisposable
@inject PersistentComponentState ApplicationState

...

@code {
    private {TYPE} data;
    private PersistingComponentStateSubscription persistingSubscription;

    protected override async Task OnInitializedAsync()
    {
        persistingSubscription = ApplicationState.RegisterOnPersisting(PersistData);

        if (!ApplicationState.TryTakeFromJson<{TYPE}>("{TOKEN}", out var restored))
        {
            data = await ...;
        }
        else
        {
            data = restored!;
        }
    }

    private Task PersistData()
    {
        ApplicationState.PersistAsJson("{TOKEN}", data);
        return Task.CompletedTask;
    }

    void IDisposable.Dispose()
    {
        persistingSubscription.Dispose();
    }
}
```

By initializing components with the same state used during prerendering, any expensive initialization steps are only executed once. The rendered UI also matches the prerendered UI, so no flicker occurs in the browser.

[*Source - Microsoft ASP.NET Core Blazor : Prerender*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/prerender)

## Generic Type Support
### Generic Type Parameter Support
The `@typeparam` directive declares a generic type parameter.

```C#
@typeparam TEntity where TEntity : IEntity
```

```C#
@typeparam TExample

@if (ExampleList is not null)
{
    <ul>
        @foreach (var item in ExampleList)
        {
            <li>@item</li>
        }
    </ul>
}

@code {
    [Parameter]
    public IEnumerable<TExample>? ExampleList{ get; set; }
}
```

### Cascaded Generic Type Support
An ancestor component can cascade a type parameter by name to descendants using the `[CascadingTypeParameter]` attribute.

```C#
@attribute [CascadingTypeParameter(nameof(TExample))]
@typeparam TExample
```

By adding @attribute `[CascadingTypeParameter(...)]` to a component, the specified generic type argument is automatically used by descendants that:
- Are nested as child content for the component in the same .razor document.
- Also declare a `@typeparam` with the exact same name.
- Don't have another value explicitly supplied or implicitly inferred for the type parameter. If another value is supplied or inferred, it takes precedence over the cascaded generic type.

When cascading the data in the following, the type must be provided to the component.

```C#
<CascadingValue Value="@stringData">
    <ListGenericTypeItems3 TExample="string">
        <ListDisplay1 />
        <ListDisplay2 />
    </ListGenericTypeItems3>
</CascadingValue>
```

[*Source - Microsoft ASP.NET Core Blazor : Generic Type Support*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/generic-type-support)

## Synchronization Context
Blazor uses a synchronization context (`SynchronizationContext`) to enforce a single logical thread of execution. A component's lifecycle methods and event callbacks raised by Blazor are executed on the synchronization context.

Blazor's server-side synchronization context attempts to emulate a single-threaded environment so that it closely matches the WebAssembly model in the browser, which is single threaded. At any given point in time, work is performed on exactly one thread, which yields the impression of a single logical thread. No two operations execute concurrently.

### Avoid Thread-blocking Calls
The following methods block the execution thread and thus block the app from resuming work until the underlying Task is complete:
- Result
- Wait
- WaitAny
- WaitAll
- Sleep
- GetResult

### Invoke Component Methods Externally to Update State
In the event a component must be updated based on an external event, such as a timer or other notification, use the `InvokeAsync` method, which dispatches code execution to Blazor's synchronization context.

When a components method is onvoked by a service outside of Blazor's synchronization context, the component can use `InvokeAsync` to switch to the correct context. 
```C#
    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            // do stuff here under the correct context...
            StateHasChanged();
        });
    }
```

[*Source - Microsoft ASP.NET Core Blazor : Synchronization Context*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/synchronization-context)

## Preserve Relationships with @key
When rendering a list of elements or components and the elements or components subsequently change, Blazor must decide which of the previous elements or components are retained and how model objects should map to them. Normally, this process is automatic and sufficient for general rendering, but there are often cases where controlling the process using the `@key` directive attribute is required.

The mapping process of elements or components to a collection can be controlled with the `@key` directive attribute. Use of `@key` guarantees the preservation of elements or components based on the key's value.

In the following example, when the people collection changes, the association between `Details` instances and `person` instances is retained. With the `Details` component keyed on the person item, Blazor ignores rerendering `Details` components that haven't changed.

```C#
<h1>People Example</h1>

@foreach (var person in people)
{
    <Details @key="person" Data="@person.Data" />
}
```

 If an `person` instance changes, the `@key` attribute directive forces Blazor to:
- Discard the entire `<Details>` and their descendants.
- Rebuild the subtree within the UI with new elements and components.

This is useful to guarantee that no UI state is preserved when the collection changes within a subtree.

Collection updates exhibit the following behavior when the `@key` directive attribute is used:
- When a `Person` is inserted at the beginning of the collection, one new `Details` instance is inserted at that corresponding position. Other instances are left unchanged.
- If an instance is deleted from the collection, only the corresponding component instance is removed from the UI. Other instances are left unchanged.
- If collection entries are re-ordered, the corresponding component instances are preserved and re-ordered in the UI.

Generally, it makes sense to supply one of the following values for `@key`:
- Model object instances. For example, the `Person` instance (`person`) was used in the example. This ensures preservation based on object reference equality.
- Unique identifiers. For example, unique identifiers can be based on primary key values of type `int`, `string`, or `Guid`.

> [!NOTE]
> The only advantage to using `@key` is control over how model instances are mapped to the preserved component instances, instead of Blazor selecting the mapping.

> [!TIP]
> Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `foreach` block) and a suitable value exists to define the `@key`.

> [!WARNING]
> There's a performance cost when rendering with `@key`. The performance cost isn't large, but only specify `@key` if preserving the element or component benefits the app.
 
[*Source - Microsoft ASP.NET Core Blazor : Preserve relationships with @key*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/element-component-model-relationships)

## Overwriting Parameters
The Blazor framework generally imposes safe parent-to-child parameter assignment:
- Parameters aren't overwritten unexpectedly.
- Side effects are minimized. For example, additional renders are avoided because they may create infinite rendering loops.

A child component receives new parameter values that possibly overwrite existing values when the parent component rerenders.

> [!NOTE]
> General guidance is not to create components that directly write to their own parameters after the component is rendered for the first time.

[*Source - Microsoft ASP.NET Core Blazor : Overwriting Parameters*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/overwriting-parameters)

## Attribute Splatting
Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the `@attributes` Razor directive attribute.

```C#
<input id="useIndividualParams"
       maxlength="@maxlength"
       placeholder="@placeholder"
       required="@required"
       size="@size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    private string maxlength = "10";
    private string placeholder = "Input placeholder text";
    private string required = "required";
    private string size = "50";

    private Dictionary<string, object> InputAttributes { get; set; } =
        new()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

[*Source - Microsoft ASP.NET Core Blazor : Attribute Splatting*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/splat-attributes-and-arbitrary-parameters)

## Layouts
A Blazor layout is a Razor component that shares markup with components that reference it. Layouts can use data binding, dependency injection, and other features of components.

A layout is a `.razor` component that inherits from `LayoutComponentBase` which defines a `Body` property (`RenderFragment` type) for the rendered content inside the layout. `@Body` specifies the location in the layout markup where the content is rendered.

### Apply a Layout to a Component
Add an `@using` directive to the `_Imports.razor` file.
```C#
@using BlazorSample.Components.Layout
```

Use the `@layout` Razor directive to apply a layout to a routable Razor component that has an `@page` directive. The compiler converts `@layout` into a `LayoutAttribute` and applies the attribute to the component class.

```C#
@page "/mypage"
@layout MyCustomLayout

<h2>Hello World!</h2>
```

### Apply a Layout to a Folder of Components
Every folder of an app can optionally contain a template file named `_Imports.razor`. The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.

### Apply a Default Layout to an App
Specify the default app layout in the `Router` component's `RouteView` component. Use the `DefaultLayout` parameter to set the layout type:
```C
<RouteView RouteData="@routeData" DefaultLayout="@typeof({LAYOUT})" />
```

> [!TIP]
> Layouts can be nested.

[*Source - Microsoft ASP.NET Core Blazor : Layout*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/layouts)

## Sections
Sections allow you to control the content in a Razor component from a child Razor component.
`SectionOutlet` renders content provided by `SectionContent` components with matching `SectionName` or `SectionId` arguments. Two or more `SectionOutlet` components can't have the same `SectionName` or `SectionId`.
```C#
<SectionOutlet SectionName="my-section-name" />
```

`SectionContent` provides content as a `RenderFragment` to `SectionOutlet` components with a matching `SectionName` or `SectionId`. If several `SectionContent` components have the same `SectionName` or `SectionId`, the matching `SectionOutlet` component renders the content of the **last** rendered `SectionContent`.
```C#
<SectionContent SectionName="my-section-name">
    <button class="btn btn-primary">Click me</button>
</SectionContent>
```

> [!WARNING]
> `SectionName` and `SectionId` are mutually exclusive. Use one or the other.

[*Source - Microsoft ASP.NET Core Blazor : Sections*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/sections)

## Control \<head\> Content
Razor components can modify the HTML `<head>` element content of a page, including setting the page's `<title>` element and modifying `<meta>` elements.

Specify the page's title with the `<PageTitle>` component, which enables rendering an HTML `<title>` element to a HeadOutlet component.

Specify `<head>` element content with the `HeadContent` component, which provides content to a `HeadOutlet` component.

```C#
<PageTitle>@title</PageTitle>

<HeadContent>
    <meta name="description" content="@description">
</HeadContent>
```

The `HeadOutlet` component renders content provided by `PageTitle` and `HeadContent` components.
```C#
<head>
    ...
    <HeadOutlet />
</head>
```

In an app created from the Blazor WebAssembly project template, the `HeadOutlet` component is added to the `RootComponents` collection of the `WebAssemblyHostBuilder` in the client-side `Program` file:
```C#
builder.RootComponents.Add<HeadOutlet>("head::after");
```

[*Source - Microsoft ASP.NET Core Blazor : Control <head> Content*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/control-head-content)
















