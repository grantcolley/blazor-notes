# blazor-notes
##### ASP.NET Core Blazor .NET 8.0

> [!IMPORTANT]
> This is a collection of notes about **ASP.NET Core Blazor** taken while reading other resources\*. The purpose of this `readme` is simply to consolidate those notes I find important or useful to me in some way, and condense them into a single page for my own personal reference.
> 
> In many cases, the notes may be straight copy and paste. In others, I may shorten or reword it, as per my understanding of the original text. In all cases, the original content is not my own and I provide links to the original source.
>
> \* *The content is mostly from [**Microsoft's Blazor documentation**](https://learn.microsoft.com/en-us/aspnet/core/blazor), with some pointers to [**Microsoft's ASP.NET Core documentation**](https://learn.microsoft.com/en-us/aspnet/core/fundamentals) and [**Mozilla MDN Web Docs**](https://developer.mozilla.org/en-US/docs/Web) for additional reading about broader web development concepts.*

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
  * [Cascading Values and Parameters](#cascading-values-and-parameters)
    * [Root-level Cascading Values](#root-level-cascading-values)
    * [CascadingValue Component](#cascadingvalue-component)
    * [\[CascadingParameter\] attribute](#cascadingparameter-attribute)
    * [Cascading Values/Parameters and Render Mode Boundaries](#cascading-valuesparameters-and-render-mode-boundaries)
    * [Cascade Multiple Values](#cascade-multiple-values)
  * [Event Handling](#event-handling)
    * [Delegate Event Handlers](#delegate-event-handlers)
    * [EventCallback](#eventcallback)
    * [Prevent Default Actions](#prevent-default-actions)
    * [Stop Event Propagation](#stop-event-propagation)
  * [Data Binding](#data-binding)
    * [Binding](#binding)
    * [Execute Asynchronous Logic After Binding](#execute-asynchronous-logic-after-binding)
    * [Two-Way Binding](#two-way-binding)
  * [Razor Component Lifecycle](#razor-component-lifecycle)
    * [Lifecycle Events](#lifecycle-events)
      * [Component Lifecycle Events](#component-lifecycle-events)
      * [The Render Lifecycle](#the-render-lifecycle)
    * [SetParametersAsync](#setparametersasync)
    * [OnInitialized{Async}](#oninitializedasync)
    * [OnParametersSet{Async}](#onparameterssetasync)
    * [OnParametersSet{Async}](#onparameterssetasync)
    * [OnAfterRender{Async}](#onafterrenderasync)
    * [StateHasChanged](#statehaschanged)
    * [Stateful Reconnection after Prerendering](#stateful-reconnection-after-prerendering)
    * [Component Disposal with IDisposable and IAsyncDisposable](#component-disposal-with-idisposable-and-iasyncdisposable)
  * [Virtualization](#virtualization)
  * [Rendering](#rendering)
    * [Rendering Conventions for ComponentBase](#rendering-conventions-for-componentbase)
    * [Streaming Rendering](#streaming-rendering)
    * [Suppress UI refreshing with ShouldRender](#suppress-ui-refreshing-with-shouldrender)
    * [When to call StateHasChanged](#when-to-call-statehaschanged)
  * [Dynamically-rendered Components](#dynamically-rendered-components)
    * [DynamicComponent](#dynamiccomponent)
    * [Event Callbacks](#event-callbacks)
* [Forms](#forms)
  * [Forms Overview](#forms-overview)
    * [HTML Forms](#html-forms)
    * [EditForm](#editform)
    * [DataAnnotations](#dataannotations)
    * [Form Submission](#form-submission)
    * [Antiforgery Support](#antiforgery-support)
  * [Input Components](#input-components)
* [Forms Binding](#forms-binding)
   * [EditForm/EditContext Model](#editFormeditContext-model)
   * [Model Binding](#model-binding)
   * [Context Binding](#context-binding)
   * [Form Names](#form-names)
* [Validation](#validation)
  * [Form Validation](#form-validation)
  * [DataAnnotationsValidator](#dataannotationsvalidator)
  * [Create a Custom Validator](#create-a-custom-validator)
  * [Server Validation with a Validator Component](#server-validation-with-a-validator-component)
  * [Validation Summary and Validation Message Components](#validation-summary-and-validation-message-components)
  * [Determine if a Form Field is Valid](#determine-if-a-form-field-is-valid)
  * [Nested Models, Collection Types, and Complex Types](#nested-models-collection-types-and-complex-types)
  * [Enable the Submit Button Based on Form Validation](#enable-the-submit-button-based-on-form-validation)
* [State Management](#state-management)
  * [Maintain User State](#maintain-user-state)
  * [Persist a State Across Circuits](#persist-a-state-across-circuits)

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
[*Source - Mozilla : How Browsers Work*](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)
<br>
[*Source - Mozilla : Cascading Style Sheet Object Model (CSSOM)*](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)
<br>
[*Source - Microsoft ASP.NET Core fundamentals overview - Web root*](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/#web-root)

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

## Cascading Values and Parameters
Cascading values and parameters provide a convenient way to flow data down a component hierarchy. Unlike `Component` parameters, cascading values and parameters don't require an attribute assignment for each descendent component where the data is consumed. Cascading values and parameters also allow components to coordinate with each other across a component hierarchy.

### Root-level Cascading Values
Root-level cascading values can be registered for the entire component hierarchy. Named cascading values and subscriptions for update notifications are supported.

```C#
builder.Services.AddCascadingValue(sp => new MyClass { Units = 123 });
builder.Services.AddCascadingValue("SecondClass", sp => new MyClass { Units = 456 });
```

```C#
@page "/page1"

@code {
    [CascadingParameter]
    public MyClass? MyClass { get; set; }

    [CascadingParameter(Name = "SecondClass")]
    public MyClass? SecondClass { get; set; }
}
```

In the following example, `MyClass` is registered as a cascading value using `CascadingValueSource<T>`, where `<T>` is the type. The `isFixed` flag indicates whether the value is fixed. If false, all recipients are subscribed for update notifications, which are issued by calling `NotifyChangedAsync`.

```C#
builder.Services.AddCascadingValue(sp =>
{
    var myClass = new MyClass { Units = 789 };
    var source = new CascadingValueSource<MyClass>(myClass, isFixed: false);
    return source;
});
```

### CascadingValue Component
A cascading value is provided using a `CascadingValue` component, which wraps a subtree of a component hierarchy and supplies a single value to all of the components within its subtree.

```C#
<CascadingValue Value="@myClass">
    <Router ...>
        <Found ...>
            ...
        </Found>
    </Router>
</CascadingValue>

@code {
    private MyClass myClass = new MyClass { Units = 789 };
}
```

### [CascadingParameter] Attribute
To make use of cascading values, descendent components declare cascading parameters using the `[CascadingParameter]` attribute. 

```C#
@code {
    [CascadingParameter]
    protected MyClass? MyClass { get; set; }
}
```

### Cascading Values/Parameters and Render Mode Boundaries

> [!WARNING]
> Cascading parameters don't pass data across render mode boundaries!
> 
> Interactive sessions run in a different context than the pages that use static server-side rendering (static SSR). There's no requirement that the server producing the page is even the same machine that hosts some later Interactive Server session, including for WebAssembly components where the server is a different machine to the client. The benefit of static server-side rendering (static SSR) is to gain the full performance of pure stateless HTML rendering.

### Cascade Multiple Values
To cascade multiple values of the same type within the same subtree, provide a unique Name string to each `CascadingValue` component and their corresponding `[CascadingParameter]` attributes.

```C#
<CascadingValue Value="@parentCascadeParameter1" Name="CascadeParam1">
    <CascadingValue Value="@ParentCascadeParameter2" Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private CascadingType? parentCascadeParameter1;

    [Parameter]
    public CascadingType? ParentCascadeParameter2 { get; set; }
}
```
And in the consuming component...
```C#
@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected CascadingType? ChildCascadeParameter1 { get; set; }

    [CascadingParameter(Name = "CascadeParam2")]
    protected CascadingType? ChildCascadeParameter2 { get; set; }
}
```

## Event Handling
### Delegate Event Handlers
Specify delegate event handlers in Razor component markup with `@on{DOM EVENT}="{DELEGATE}"`.
For event handling:
- Asynchronous delegate event handlers that return a `Task` are supported.
- Delegate event handlers automatically trigger a UI render, so there's no need to manually call `StateHasChanged`.
- Exceptions are logged.
```C#
@page "/event-handler"

<h2>@headingValue</h2>

<button @onclick="UpdateHeading">Update heading</button>

@code {
    private async Task UpdateHeading()
    {
        await Task.Delay(2000);
        headingValue = $"New heading ({DateTime.Now})";
    }
}
```

For events that support an event argument type, specifying an event parameter in the event method definition is only necessary if the event type is used in the method.
```C#
@page "/event-handler"

<button @onclick="ReportPointerLocation">Where's my mouse pointer for this button?</button>

<p>@mousePointerMessage</p>

@code {
    private string? mousePointerMessage;

    private void ReportPointerLocation(MouseEventArgs e)
    {
        mousePointerMessage = $"Mouse coordinates: {e.ScreenX}:{e.ScreenY}";
    }
}
```

Lambda expressions are supported as the delegate event handler.
```C#
@page "/event-handler"

<h2>@heading</h2>

<button @onclick="@(e => heading = "New heading!!!")">Update heading</button>

@code {
    private string heading = "Initial heading";
}
```

### EventCallback
To expose events across components, use an `EventCallback`. The following child component demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's parent component.
```C#
<button @onclick="OnClickCallback">Trigger a Parent component method.</button>

@code {
    [Parameter]
    public EventCallback<MouseEventArgs> OnClickCallback { get; set; }
}
```

```C#
@page "/parent-child"

<Child OnClickCallback="@ShowMessage">Show message from a child component.</Child>

<p>@message</p>

@code {
    private string? message;

    private void ShowMessage(MouseEventArgs e)
    {
        message = $"Blaze a new trail with Blazor! ({e.ScreenX}:{e.ScreenY})";
    }
}
```

> [!TIP]
> Prefer the strongly typed `EventCallback<TValue>` over `EventCallback`. `EventCallback<TValue>` provides enhanced error feedback to users of the component. Similar to other UI event handlers, specifying the event parameter is optional. Use `EventCallback` when there's no value passed to the callback.

`EventCallback` and `EventCallback<TValue>` permit asynchronous delegates.

```C#
<h3>Child2 Component</h3>

<button @onclick="TriggerEvent">Click Me</button>

@code {
    [Parameter]
    public EventCallback<string> OnClickCallback { get; set; }

    private async Task TriggerEvent()
    {
        await OnClickCallback.InvokeAsync("Blaze It!");
    }
}
```

```C#
@page "/parent-child"

<Child OnClickCallback="@(async (value) => { await Task.Yield(); messageText = value; })" />

<p>@messageText</p>

@code {
    private string messageText = string.Empty;
}
```

### Prevent Default Actions
Use the `@on{DOM EVENT}:preventDefault` directive attribute to prevent the default action for an event.

```C#
@page "/event-handler"

<input value="@count" @onkeydown="KeyHandler" @onkeydown:preventDefault />

@code {
    private void KeyHandler(KeyboardEventArgs e)
    {
    }
}
```

### Stop Event Propagation
Use the `@on{DOM EVENT}:stopPropagation` directive attribute to stop event propagation within the Blazor scope.

```C#
@page "/event-handler"

<div @onclick="OnSelectChildDiv" @onclick:stopPropagation=true>
    Child div that stops propagation.
</div>
```

[*Source - Microsoft ASP.NET Core Blazor : Cascading Values and Parameters*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/cascading-values-and-parameters)
<br>
[*Source - Mozilla : DOM event*](https://developer.mozilla.org/en-US/docs/Web/Events)

## Data Binding
### Binding
Razor components provide data binding features with the `@bind` Razor directive attribute with a field, property, or Razor expression value. The text box is updated in the UI only when the component is rendered, not in response to changing the field's or property's value. Since components render themselves after event handler code executes, field and property updates are usually reflected in the UI immediately after an event handler is triggered.
<br>
In the following example, when the user enters a value in the text box and changes element focus, the `onchange` event is fired and the `inputValue` property is set to the changed value.
```C#
<input @bind="inputValue" />

@code {
    private string? inputValue;
}
```

### Execute Asynchronous Logic After Binding
To execute asynchronous logic after binding, use `@bind:after="{EVENT}"`. An assigned C# method isn't executed until the bound value is assigned synchronously. 

Using an event callback parameter (`EventCallback`/`EventCallback<T>`) with `@bind:after` isn't supported. Instead, pass a method that returns an `Action` or `Task` to `@bind:after`.

In the following example:
- The `<input>` element's value is bound to the value of `searchText` synchronously.
- After each keystroke (`onchange` event) in the field, the `PerformSearch` method executes asynchronously.
- `PerformSearch` calls a service with an asynchronous method (`FetchAsync`) to return search results.
```C#
@inject ISearchService SearchService

<input @bind="searchText" @bind:after="PerformSearch" />

@code {
    private string? searchText;
    private string[]? searchResult;

    private async Task PerformSearch()
    {
        searchResult = await SearchService.FetchAsync(searchText);
    }
}
```

### Two-Way Binding
In **ASP.NET Core 7.0** or later, `@bind:get`/`@bind:set` modifier syntax is used to implement two-way data binding.
- `@bind:get:` Specifies the value to bind.
- `@bind:set:` Specifies a callback for when the value changes.

> [!NOTE]
> The `@bind:get` and `@bind:set` modifiers are always used together.

```C#
<p>
    <input @bind:event="oninput" @bind:get="inputValue" @bind:set="OnInput" />
</p>

<p>
    <code>inputValue</code>: @inputValue
</p>

@code {
    private string? inputValue;

    private void OnInput(string value)
    {
        var newValue = value ?? string.Empty;

        inputValue = newValue.Length > 4 ? "Long!" : newValue;
    }
}
```

[*Source - Microsoft ASP.NET Core Blazor : Data Binding*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/data-binding)

## Razor Component Lifecycle
### Lifecycle Events
#### Component Lifecycle Events
1. If the component is rendering for the first time on a request:
    -  Create the component's instance.
    -  Perform property injection. Run `SetParametersAsync`.
    - Call `OnInitialized{Async}`. If an incomplete `Task` is returned, the `Task` is awaited and then the component is rerendered.
2. Call `OnParametersSet{Async}`. If an incomplete `Task` is returned, the `Task` is awaited and then the component is rerendered.
3. Render for all synchronous work and complete `Tasks`.

#### The Render Lifecycle
1. Avoid further rendering operations on the component:
    - After the first render.
    - When ShouldRender is false.
2. Build the render tree diff (difference) and render the component.
3. Await the DOM to update.
4. Call OnAfterRender{Async}.

> [!NOTE]
> A parent component renders before its children components because rendering is what determines which children are present. If synchronous parent component initialization is used, the parent initialization is guaranteed to complete first. If asynchronous parent component initialization is used, the completion order of parent and child component initialization can't be determined because it depends on the initialization code running.

> [!TIP]
> When overriding Blazor's lifecycle methods, it isn't necessary to call base class lifecycle methods for `ComponentBase`. However, a component should call an overridden base class lifecycle method if the base class method contains logic that must be executed. 

### SetParametersAsync
`SetParametersAsync` sets parameters supplied by the component's parent in the render tree or from route parameters.

### OnInitialized{Async}
`OnInitialized` and `OnInitializedAsync` are invoked when the component is initialized after having received its initial parameters in `SetParametersAsync`.

Blazor apps that prerender their content on the server call `OnInitializedAsync` twice:
   - Once when the component is initially rendered statically as part of the page.
   - A second time when the browser renders the component.

> [!WARNING]
> While a Blazor app is prerendering, certain actions, such as calling into `JavaScript (JS interop)`, aren't possible.

> [!TIP]
> Use streaming rendering with Interactive Server components to improve the user experience for components that perform long-running asynchronous tasks in `OnInitializedAsync` to fully render.

### OnParametersSet{Async}
`OnParametersSet` or `OnParametersSetAsync` are called after the component is initialized in `OnInitialized` or `OnInitializedAsync`.

### OnAfterRender{Async}
`OnAfterRender` and `OnAfterRenderAsync` are invoked after a component has rendered interactively and the UI has finished updating (for example, after elements are added to the browser DOM).

Use this stage to perform additional initialization steps with the rendered content, such as `JS interop` calls that interact with the rendered `DOM elements`.

For `OnAfterRenderAsync`, the component doesn't automatically rerender after the completion of any returned `Task` to avoid an infinite render loop.
The `firstRender` parameter for `OnAfterRender` and `OnAfterRenderAsync`:
   - Is set to true the first time that the component instance is rendered.
   - Can be used to ensure that initialization work is only performed once.

> [!NOTE]
> `OnAfterRender` and `OnAfterRenderAsync` aren't called during the prerendering process on the server. The methods are called when the component is rendered interactively after prerendering.

### StateHasChanged
Calling `StateHasChanged` notifies the component that its state has changed causing the component to be rerendered.

`StateHasChanged` is called automatically for `EventCallback` methods.

### Stateful Reconnection after Prerendering
When prerendering on the server, a component is initially rendered statically as part of the page. Once the browser establishes a `SignalR` connection back to the server, the component is rendered again and interactive. If the `OnInitialized{Async}` lifecycle method for initializing the component is present, the method is executed twice:
- When the component is prerendered statically.
- After the server connection has been established.

### Component Disposal with IDisposable and IAsyncDisposable

`IDisposable`
```C#
@implements IDisposable

@code {
    public void Dispose()
    {
        obj?.Dispose();
    }
}
```

`IAsyncDisposable`
```C#
@implements IAsyncDisposable

@code {
    public async ValueTask DisposeAsync()
    {
        if (obj is not null)
        {
            await obj.DisposeAsync();
        }
    }
}
```

> [!NOTE]
> Components shouldn't need to implement `IDisposable` and `IAsyncDisposable` simultaneously. If both are implemented, the framework only executes the asynchronous overload.

> [!WARNING]
> Always unsubscribe event handlers from .NET events.

[*Source - Microsoft ASP.NET Core Blazor : Lifecycle*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/lifecycle)

## Virtualization
Virtualization is a technique for limiting UI rendering to just the parts that are currently visible using the the `Virtualize<TItem>` component, thereby improving the perceived performance of component rendering.

Use the `Virtualize<TItem>` component when:
- Rendering a set of data items in a loop.
- Most of the items aren't visible due to scrolling.
- The rendered items are the same size.

When the user scrolls to an arbitrary point in the `Virtualize<TItem>` component's list of items, the component calculates the visible items to show. Unseen items aren't rendered.

```C#
<div style="height:500px;overflow-y:scroll" tabindex="-1">
    <Virtualize Items="@allFlights" Context="flight">
        <FlightSummary @key="flight.FlightId" Details="@flight.Summary" />
    </Virtualize>
</div>
```

> [!TIP]
> Keyboard scroll support.
> 
> To allow users to scroll virtualized content using their keyboard, ensure that the virtualized elements or scroll container itself is focusable. If you fail to take this step, keyboard scrolling doesn't work in Chromium-based browsers.
>
> The example above uses a `tabindex` attribute on the scroll container.

[*Source - Microsoft ASP.NET Core Blazor : Virtualization*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/virtualization)

## Rendering
### Rendering Conventions for ComponentBase
By default, Razor components inherit from the `ComponentBase` base class, which contains logic to trigger rerendering at the following times:
- After applying an updated set of parameters from a parent component.
- After applying an updated value for a cascading parameter.
- After notification of an event and invoking one of its own event handlers.
- After a call to its own `StateHasChanged` method.

### Streaming Rendering
Use *streaming rendering* with interactive server-side rendering (interactive SSR) to stream content updates on the response stream and improve the user experience for components that perform long-running asynchronous tasks to fully render.

To improve the user experience, streaming rendering initially renders the entire page quickly with placeholder content while asynchronous operations execute. After the operations are complete, the updated content is sent to the client on the same response connection and patched into the DOM.

Streaming rendering requires the server to avoid buffering the output. The response data must flow to the client as the data is generated.

To stream content updates when using static server-side rendering (static SSR), apply the `[StreamRendering(true)]` attribute to the component. Streaming rendering must be explicitly enabled because streamed updates may cause content on the page to shift.

```C#
@page "/weather"
@attribute [StreamRendering(true)]
```

### Suppress UI refreshing with ShouldRender
`ShouldRender` is called each time a component is rendered, and it can be used to manage UI refreshing.
```C#
@page "/control-render"

@code {
    private bool shouldRender = true;

    protected override bool ShouldRender()
    {
        return shouldRender;
    }
}
```

### When to call StateHasChanged
Calling `StateHasChanged` allows you to trigger a render at any time.

Code shouldn't need to call `StateHasChanged` when:
- Routinely handling events, whether synchronously or asynchronously, since ComponentBase triggers a render for most routine event handlers.
- Implementing typical lifecycle logic, such as OnInitialized or OnParametersSetAsync, whether synchronously or asynchronously, since ComponentBase triggers a render for typical lifecycle events.

However, it might make sense to call `StateHasChanged` in the following cases:
- An asynchronous handler involves multiple asynchronous phases
- Receiving a call from something external to the Blazor rendering and event handling system
- To render component outside the subtree that is rerendered by a particular event

> [!NOTE]
> Due to the way that tasks are defined in .NET, a receiver of a `Task` can only observe its final completion, not intermediate asynchronous states. Therefore, `ComponentBase` can only trigger rerendering when the `Task` is first returned and when the `Task` finally completes. The framework can't know to rerender a component at other intermediate points.

[*Source - Microsoft ASP.NET Core Blazor : Rendering*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/rendering)

## Dynamically-rendered Components
### DynamicComponent
`DynamicComponent` component allows you to render components by type, which is useful for rendering components without iterating through possible types or using conditional logic. For example, `DynamicComponent` can render a component based on a user selection from a dropdown list.

In the following example:
- `componentType` specifies the type.
- parameters specifies component parameters to pass to the `componentType` component, in the form of a `IDictionary<string, object>`.
  
```C#
<DynamicComponent Type="@componentType" Parameters="@parameters" />

@code {
    private Type componentType = ...;
    private IDictionary<string, object> parameters = ...;
}
```

### Event Callbacks
Event callbacks `EventCallback` can be passed to a `DynamicComponent` in its parameter dictionary.

`RocketLab.razor`
```C#
<h2>Rocket Lab®</h2>

<button @onclick="OnClickCallback">Trigger a Parent component method</button>

@code {
    [Parameter]
    public EventCallback<MouseEventArgs> OnClickCallback { get; set; }
}
```

`SpaceX.razor`
```C#
<h2>SpaceX®</h2>

<button @onclick="OnClickCallback">Trigger a Parent component method</button>

@code {
    [Parameter]
    public EventCallback<MouseEventArgs> OnClickCallback { get; set; }
}
```

`DynamicComponent.razor`
```C#
@page "/dynamic-component"

<label>
    Select your transport:
    <select @onchange="OnDropdownChange">
        <option value="">Select a value</option>
        <option value="@nameof(RocketLab2)">Rocket Lab</option>
        <option value="@nameof(SpaceX2)">SpaceX</option>
    </select>
</label>

@if (selectedType is not null)
{
    <div class="border border-primary my-1 p-1">
        <DynamicComponent Type="@selectedType" Parameters="@Components[selectedType.Name].Parameters" />
    </div>
}

<p>
    @message
</p>

@code {
    private Type? selectedType;
    private string? message;

    private Dictionary<string, ComponentMetadata> Components
    {
        get
        {
            return new Dictionary<string, ComponentMetadata>()
            {
                {
                    "RocketLab",
                    new ComponentMetadata
                    {
                        Name = "Rocket Lab",
                        Parameters =
                            new()
                            {
                                {
                                    "OnClickCallback",
                                    EventCallback.Factory.Create<MouseEventArgs>(
                                        this, ShowDTMessage)
                                }
                            }
                    }
                },
                {
                    "VirginGalactic",
                    new ComponentMetadata
                    {
                        Name = "Virgin Galactic",
                        Parameters =
                            new()
                            {
                                {
                                    "OnClickCallback",
                                    EventCallback.Factory.Create<MouseEventArgs>(
                                        this, ShowDTMessage)
                                }
                            }
                    }
                }
            };
        }
    }

    private void OnDropdownChange(ChangeEventArgs e)
    {
        selectedType = Type.GetType($"BlazorSample.Components.{e.Value}");
    }

    private void ShowDTMessage(MouseEventArgs e) =>
        message = $"The current DT is: {DateTime.Now}.";
}
```

[*Source - Microsoft ASP.NET Core Blazor : DynamicComponent*](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/dynamiccomponent)

# Forms
## Forms Overview
> [!NOTE]
> Built-in input components can be ound to an object or model that can use data annotations.

### HTML Forms
Standard HTML forms are supported. Create a form using the normal HTML `<form>` tag and specify an `@onsubmit` handler for handling the submitted form request.

```C#
@page "/starship-plain-form"

<form method="post" @onsubmit="Submit" @formname="starship-plain-form">
    <AntiforgeryToken />
    <label>
        Identifier: 
        <InputText @bind-Value="Model!.Id" />
    </label>
    <button type="submit">Submit</button>
</form>

@code {
    [SupplyParameterFromForm]
    public Starship? Model { get; set; }

    protected override void OnInitialized() => Model ??= new();

    private void Submit()
    {
        Logger.LogInformation("Id = {Id}", Model?.Id);
    }

    public class Starship
    {
        public string? Id { get; set; }
    }
}
```

> [!TIP]
> The `[SupplyParameterFromForm]` attribute indicates that the value of the associated property should be supplied from the form data. Data in the request that matches the property's name is bound to the property.

Blazor enhances page navigation and form handling by intercepting the request in order to apply the response to the existing DOM, preserving as much of the rendered form as possible. The enhancement avoids the need to fully load the page and provides a much smoother user experience, similar to a single-page app (SPA), although the component is rendered on the server.

> [!NOTE]
> `Streaming` rendering is supported for plain HTML forms.

> [!IMPORTANT]
> For an HTML form, always use the `@formname` attribute directive to assign the form's name.

### EditForm
Blazor also has built-in form support using the framework's `EditForm` component.

```C#
@page "/starship"

<EditForm Model="@Model" OnSubmit="@Submit" FormName="Starship1">
    <label>
        Identifier: 
        <InputText @bind-Value="Model!.Id" />
    </label>
    <button type="submit">Submit</button>
</EditForm>

@code {
    [SupplyParameterFromForm]
    public Starship? Model { get; set; }

    protected override void OnInitialized() => Model ??= new();

    private void Submit()
    {
        Logger.LogInformation("Id = {Id}", Model?.Id);
    }

    public class Starship
    {
        public string? Id { get; set; }
    }
}
```

Blazor enhances page navigation and form handling for EditForm components.

### DataAnnotations

The following example uses `DataAnnotations`.

`OnSubmit` is replaced with `OnValidSubmit`, a `ValidationSummary` component is added to display validation messages when the form is invalid, and a `DataAnnotationsValidator` component attaching validation support using data annotations.

```C#
@page "/starship-"
@using System.ComponentModel.DataAnnotations

<EditForm Model="@Model" OnValidSubmit="@Submit" FormName="Starship2">
    <DataAnnotationsValidator />
    <ValidationSummary />
    <label>
        Identifier: 
        <InputText @bind-Value="Model!.Id" />
    </label>
    <button type="submit">Submit</button>
</EditForm>

@code {
    [SupplyParameterFromForm]
    public Starship? Model { get; set; }

    protected override void OnInitialized() => Model ??= new();

    private void Submit()
    {
        Logger.LogInformation("Id = {Id}", Model?.Id);
    }

    public class Starship
    {
        [Required]
        [StringLength(10, ErrorMessage = "Id is too long.")]
        public string? Id { get; set; }
    }
}
```

### Form Submission
`EditForm` provides the following callbacks for handling form submission:
- `OnValidSubmit` runs when a form with valid fields is submitted.
- `OnInvalidSubmit` runs when a form with invalid fields is submitted.
- `OnSubmit` runs regardless of the form fields' validation status. The form is validated by calling `EditContext.Validate` in the event handler method. If `Validate` returns true, the form is valid.

### Antiforgery Support
The `AntiforgeryToken` component renders an antiforgery token as a hidden field, and the `[RequireAntiforgeryToken]` attribute enables antiforgery protection. If an antiforgery check fails, a `400 - Bad Request response` is thrown and the form isn't processed.

`EditForm`, automatically adds the `AntiforgeryToken` component and `[RequireAntiforgeryToken]` attribute to provide antiforgery protection by default.

Forms using the HTML `<form>` element must manually add the `AntiforgeryToken` component to the form.

[*Source - Microsoft ASP.NET Core Blazor : Forms Overview*](https://learn.microsoft.com/en-us/aspnet/core/blazor/forms)

## Input Components
The Blazor framework provides built-in input components to receive and validate user input supported in an `EditForm` with an `EditContext`. Inputs are validated when they're changed and when a form is submitted.

Input component and what they are rendered as:
- InputCheckbox `<input type="checkbox">`
- InputDate<TValue>	`<input type="date">`
- InputFile	`<input type="file">`
- InputNumber<TValue>	`<input type="number">`
- InputRadio<TValue>	`<input type="radio">`
- InputRadioGroup<TValue>	Group of child `InputRadio<TValue>`
- InputSelect<TValue>	`<select>`
- InputText	`<input>`
- InputTextArea `<textarea>`

[*Source - Microsoft ASP.NET Core Blazor : Input Components*](https://learn.microsoft.com/en-us/aspnet/core/blazor/forms/input-components)

## Forms Binding
### EditForm/EditContext Model
An `EditForm` creates an `EditContext` based on the assigned object as a cascading value for other components in the form. The `EditContext` tracks metadata about the edit process, including which form fields have been modified and the current validation messages. Assigning to either an `EditForm.Model` or an `EditForm.EditContext` can bind a form to data.

> [!WARNING]
> `EditContext` or `EditForm` are mutually exclusive so assign a model to either one or a runtime error is thrown.

### Model Binding
```C#
<EditForm ... Model="@Model" ...>
    ...
</EditForm>

@code {
    [SupplyParameterFromForm]
    public Starship? Model { get; set; }

    protected override void OnInitialized() => Model ??= new();
}
```

### Context Binding
```C#
<EditForm ... EditContext="@editContext" ...>
    ...
</EditForm>

@code {
    private EditContext? editContext;

    [SupplyParameterFromForm]
    public Starship? Model { get; set; }

    protected override void OnInitialized()
    {
        Model ??= new();
        editContext = new(Model);
    }
}
```

### Form Names
Use the `FormName` parameter to assign a form name. Form names must be unique to bind model data.

```C#
<EditForm ... FormName="abc">
    ...
</EditForm>
```

Supplying a form name is required for all forms that are submitted by statically-rendered server-side components but not for forms that are submitted by interactively-rendered components. However, the recommendation is to supply a unique form name for every form to prevent runtime form posting errors if interactivity is ever dropped for a form.

> [!NOTE]
> The form name is only checked when the form is posted to an endpoint as a traditional `HTTP POST` request from a statically-rendered server-side component to ensure form names don't collide and events are routed to the correct form for form `POST` events.

[*Source - Microsoft ASP.NET Core Blazor : Binding*](https://learn.microsoft.com/en-us/aspnet/core/blazor/forms/binding)

## Validation
### Form Validation
In basic form validation scenarios, an `EditForm` instance can use declared `EditContext` and `ValidationMessageStore` instances to validate form fields. A handler for the `OnValidationRequested` event of the `EditContext` executes custom validation logic. The handler's result updates the `ValidationMessageStore` instance.

```C#
@page "/starship"
@implements IDisposable

<EditForm EditContext="editContext" OnValidSubmit="@Submit" FormName="Starship8">
    <div>
        <label>
            <InputCheckbox @bind-Value="Model!.Subsystem1" />
            Safety Subsystem
        </label>
    </div>
    <div>
        <label>
            <InputCheckbox @bind-Value="Model!.Subsystem2" />
            Emergency Shutdown Subsystem
        </label>
    </div>
    <div>
        <ValidationMessage For="() => Model!.Options" />
    </div>
    <div>
        <button type="submit">Update</button>
    </div>
</EditForm>

@code {
    private EditContext? editContext;

    [SupplyParameterFromForm]
    public Holodeck? Model { get; set; }

    private ValidationMessageStore? messageStore;

    protected override void OnInitialized()
    {
        Model ??= new();
        editContext = new(Model);
        editContext.OnValidationRequested += HandleValidationRequested;
        messageStore = new(editContext);
    }

    private void HandleValidationRequested(object? sender,
        ValidationRequestedEventArgs args)
    {
        messageStore?.Clear();

        // Custom validation logic
        if (!Model!.Options)
        {
            messageStore?.Add(() => Model.Options, "Select at least one.");
        }
    }

    private void Submit()
    {
        Logger.LogInformation("Submit called: Processing the form");
    }

    public class Holodeck
    {
        public bool Subsystem1 { get; set; }
        public bool Subsystem2 { get; set; }
        public bool Options => Subsystem1 || Subsystem2;
    }

    public void Dispose()
    {
        if (editContext is not null)
        {
            editContext.OnValidationRequested -= HandleValidationRequested;
        }
    }
}
```

### DataAnnotationsValidator
The `DataAnnotationsValidator` component attaches data annotations validation to a cascaded `EditContext`.

Blazor performs **field validation** when the user tabs out of a field, and **model validation** when the user submits the form.

### Create a Custom Validator
Inherit from `ComponentBase`. The form's `EditContext` is a cascading parameter of the component. When the validator component is initialized, a new `ValidationMessageStore` is created to maintain a current list of form errors. The message store receives errors when developer code in the form's component calls the `DisplayErrors` method, passing in a `Dictionary<string, List<string>>` containing the errors, where the key is the name of the form field that has one or more errors.

Clearing messages:
- All errors are cleared when validation is requested on the `EditContext` and the `OnValidationRequested` event is raised.
- Field errors are cleared when a field changes in the form and the `OnFieldChanged` event is raised.
- All errors are cleared when the `ClearErrors` method is called by developer code.

```C#
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Forms;

namespace BlazorSample;

public class CustomValidation : ComponentBase
{
    private ValidationMessageStore? messageStore;

    [CascadingParameter]
    private EditContext? CurrentEditContext { get; set; }

    protected override void OnInitialized()
    {
        if (CurrentEditContext is null)
        {
            throw new InvalidOperationException(
                $"{nameof(CustomValidation)} requires a cascading " +
                $"parameter of type {nameof(EditContext)}. " +
                $"For example, you can use {nameof(CustomValidation)} " +
                $"inside an {nameof(EditForm)}.");
        }

        messageStore = new(CurrentEditContext);

        CurrentEditContext.OnValidationRequested += (s, e) => 
            messageStore?.Clear();
        CurrentEditContext.OnFieldChanged += (s, e) => 
            messageStore?.Clear(e.FieldIdentifier);
    }

    public void DisplayErrors(Dictionary<string, List<string>> errors)
    {
        if (CurrentEditContext is not null)
        {
            foreach (var err in errors)
            {
                messageStore?.Add(CurrentEditContext.Field(err.Key), err.Value);
            }

            CurrentEditContext.NotifyValidationStateChanged();
        }
    }

    public void ClearErrors()
    {
        messageStore?.Clear();
        CurrentEditContext?.NotifyValidationStateChanged();
    }
}
```

### Server Validation with a Validator Component
Server validation is supported in addition to client validation where the `EditContext.Model` to a backend server API for processing model validation on the server. The server API includes both the built-in framework data annotations validation and custom validation logic supplied by the developer. If validation passes on the server, process the form and send back a `success status code (200 - OK)`. If validation fails, return a `failure status code (400 - Bad Request)` and the field validation errors.

> [!TIP]
> To support model validation on the backend server API, the model should be in a shared class library that can be referenced by the client and server api projects. Furthermore, since the model requires data annotations, the shared class library must reference the `System.ComponentModel.Annotations` package.

### Validation Summary and Validation Message Components
The `ValidationSummary` component summarizes all validation messages.
```C#
<ValidationSummary Model="@Model" />
```

The `ValidationMessage<TValue>` component displays validation messages for a specific field.
```C#
<ValidationMessage For="@(() => Model!.MaximumAccommodation)" />
```

### Determine if a Form Field is Valid
Use `EditContext.IsValid(fieldIdentifier)` to determine if a field is valid without obtaining validation messages.
```C
var isValid = editContext.IsValid(fieldIdentifier);
```

### Nested Models, Collection Types, and Complex Types
`DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection or complex-type properties.

To validate the bound model's entire object graph, including collection and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the experimental `Microsoft.AspNetCore.Components.DataAnnotations.Validation` package:

```C#
<EditForm ...>
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Annotate model properties with `[ValidateComplexType]`. 

```C#
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; } = new();
}
```

### Enable the Submit Button Based on Form Validation
Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.

```C#
@page "/starship"
@implements IDisposable
@inject ILogger<Starship14> Logger

<EditForm EditContext="@editContext" OnValidSubmit="@Submit" FormName="Starship14">
    <DataAnnotationsValidator />
    <ValidationSummary />
    <div>
        <label>
            Identifier:
            <InputText @bind-Value="Model!.Id" />
        </label>
    </div>
    <div>
        <button type="submit" disabled="@formInvalid">Submit</button>
    </div>
</EditForm>

@code {
    private bool formInvalid = false;
    private EditContext? editContext;

    [SupplyParameterFromForm]
    private Starship? Model { get; set; }

    protected override void OnInitialized()
    {
        Model ??=
            new()
                {
                    Id = "NCC-1701",
                    Classification = "Exploration"
                };
        editContext = new(Model);
        editContext.OnFieldChanged += HandleFieldChanged;
    }

    private void HandleFieldChanged(object? sender, FieldChangedEventArgs e)
    {
        if (editContext is not null)
        {
            formInvalid = !editContext.Validate();
            StateHasChanged();
        }
    }

    private void Submit()
    {
        Logger.LogInformation("Submit called: Processing the form");
    }

    public void Dispose()
    {
        if (editContext is not null)
        {
            editContext.OnFieldChanged -= HandleFieldChanged;
        }
    }
}
```

[*Source - Microsoft ASP.NET Core Blazor : Validation*](https://learn.microsoft.com/en-us/aspnet/core/blazor/forms/validation)

## State Management
### Maintain User State
Server-side Blazor is a stateful app framework. Most of the time, the app maintains a connection to the server. The user's state is held in the server's memory in a ***circuit***.

If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit with their original state, however this isn't always possible. When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.

### Persist a State Across Circuits
To preserve state across circuits, the app must persist the data to some other storage location than the server's memory.

### Where to Persist State
#### Server-side Storage
Store in a database, or some other "external" persistent storage.

#### URL
Store as part of the url

#### Browser Storage
- `localStorage` is scoped to the browser's window. Data persists in `localStorage` until explicitly cleared.
- `sessionStorage` is scoped to the browser's tab. Each tab has its own independent version of the data.
- `ProtectedLocalStorage` leverages **ASP.NET Core Data Protection** for `localStorage`
- `ProtectedSessionStorage` leverages **ASP.NET Core Data Protection** for `sessionStorage`

> [!TIP]
> Generally, `sessionStorage` is safer to use than `localStorage` because it avoids the risk that a user opens multiple tabs and encounters issues sharing state across tabs.
>
> `localStorage` is the better choice if the app must persist state across closing and reopening the browser.

> [!NOTE]
> Protected Browser Storage relies on **ASP.NET Core Data Protection** and is only supported for server-side Blazor apps.

```C#
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

#### In-memory State Container Service



