---
title: Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78649085"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="5100e-103">Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="5100e-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="5100e-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="5100e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="5100e-105">Los componentes Razor se pueden integrar en aplicaciones de Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="5100e-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="5100e-106">Cuando se representa la página o la vista, los componentes se pueden representar previamente al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="5100e-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="5100e-107">Preparación de la aplicación para usar componentes en páginas y vistas</span><span class="sxs-lookup"><span data-stu-id="5100e-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="5100e-108">Una aplicación Razor Pages o MVC existente puede integrar componentes Razor en páginas y vistas:</span><span class="sxs-lookup"><span data-stu-id="5100e-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="5100e-109">En el archivo de diseño de la aplicación ( *_Layout.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="5100e-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="5100e-110">Agregue la etiqueta `<base>` siguiente al elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="5100e-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="5100e-111">El valor `href` (la *ruta de acceso base de la aplicación*) del ejemplo anterior da por hecho que la aplicación reside en la ruta de acceso URL raíz (`/`).</span><span class="sxs-lookup"><span data-stu-id="5100e-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="5100e-112">Si la aplicación es una subaplicación, siga las instrucciones de la sección *Ruta de acceso base de la aplicación* del artículo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="5100e-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="5100e-113">El archivo *_Layout.cshtml* se encuentra en la carpeta *Pages/Shared* en una aplicación Razor Pages o en la carpeta *Views/Shared* en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="5100e-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="5100e-114">Agregue una etiqueta `<script>` para el script *blazor.server.js* inmediatamente antes de la etiqueta `</body>` de cierre:</span><span class="sxs-lookup"><span data-stu-id="5100e-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="5100e-115">El marco agrega el script *blazor.server.js* a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5100e-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="5100e-116">No es necesario agregar manualmente el script a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5100e-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="5100e-117">Agregue un archivo *_Imports.razor* a la carpeta raíz del proyecto con el contenido siguiente (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):</span><span class="sxs-lookup"><span data-stu-id="5100e-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="5100e-118">En `Startup.ConfigureServices`, registre el servicio Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="5100e-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="5100e-119">En `Startup.Configure`, agregue el punto de conexión del centro Blazor a `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="5100e-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="5100e-120">Integre los componentes en cualquier página o vista.</span><span class="sxs-lookup"><span data-stu-id="5100e-120">Integrate components into any page or view.</span></span> <span data-ttu-id="5100e-121">Para obtener más información, vea la sección [Representación de componentes a partir de una página o vista](#render-components-from-a-page-or-view).</span><span class="sxs-lookup"><span data-stu-id="5100e-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="5100e-122">Uso de componentes enrutables en una aplicación Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5100e-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="5100e-123">*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="5100e-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="5100e-124">Para admitir componentes Razor enrutables en aplicaciones Razor Pages, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="5100e-125">Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="5100e-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="5100e-126">Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-126">Add an *App.razor* file to the project root with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="5100e-127">Agregue un archivo *_Host.cshtml* a la carpeta *Pages* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="5100e-128">Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="5100e-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="5100e-129">Agregue una ruta de prioridad baja para la página *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5100e-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="5100e-130">Agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5100e-130">Add routable components to the app.</span></span> <span data-ttu-id="5100e-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5100e-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="5100e-132">Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="5100e-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="5100e-133">Uso de componentes enrutables en una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="5100e-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="5100e-134">*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="5100e-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="5100e-135">Para admitir componentes Razor enrutables en aplicaciones MVC, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="5100e-136">Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="5100e-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="5100e-137">Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="5100e-138">Agregue un archivo *_Host.cshtml* a la carpeta *Views/Home* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="5100e-139">Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="5100e-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="5100e-140">Agregue una acción al controlador de inicio:</span><span class="sxs-lookup"><span data-stu-id="5100e-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="5100e-141">Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5100e-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="5100e-142">Cree una carpeta *Pages* y agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5100e-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="5100e-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5100e-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="5100e-144">Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="5100e-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="5100e-145">Espacios de nombres de componentes</span><span class="sxs-lookup"><span data-stu-id="5100e-145">Component namespaces</span></span>

<span data-ttu-id="5100e-146">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta a la página o vista, o al archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5100e-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="5100e-147">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5100e-147">In the following example:</span></span>

* <span data-ttu-id="5100e-148">Cambie `MyAppNamespace` en el espacio de nombres de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5100e-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="5100e-149">Si no se usa una carpeta denominada *Components* para contener los componentes, cambie `Components` en la carpeta donde estos se encuentren.</span><span class="sxs-lookup"><span data-stu-id="5100e-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="5100e-150">El archivo *_ViewImports.cshtml* se encuentra en la carpeta *Pages* de una aplicación Razor Pages o en la carpeta *Views* de una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="5100e-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="5100e-151">Para obtener más información, vea <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="5100e-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="5100e-152">Representación de componentes a partir de una página o vista</span><span class="sxs-lookup"><span data-stu-id="5100e-152">Render components from a page or view</span></span>

<span data-ttu-id="5100e-153">*Esta sección pertenece a la adición de componentes a páginas o vistas, donde los componentes no son enrutables directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="5100e-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="5100e-154">Para representar un componente a partir de una página o vista, use el Asistente de etiquetas `Component`:</span><span class="sxs-lookup"><span data-stu-id="5100e-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="5100e-155">El tipo de parámetro debe ser JSON serializable, lo que normalmente significa que el tipo debe tener un constructor predeterminado y propiedades configurables.</span><span class="sxs-lookup"><span data-stu-id="5100e-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="5100e-156">Por ejemplo, se puede especificar un valor para `IncrementAmount` porque el tipo de `IncrementAmount` es `int`, que es un tipo primitivo compatible con el serializador JSON.</span><span class="sxs-lookup"><span data-stu-id="5100e-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="5100e-157">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="5100e-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="5100e-158">Se representa previamente en la página.</span><span class="sxs-lookup"><span data-stu-id="5100e-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="5100e-159">Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="5100e-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="5100e-160">Descripción</span><span class="sxs-lookup"><span data-stu-id="5100e-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="5100e-161">Representa el componente en código HTML estático e incluye un marcador para una aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5100e-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="5100e-162">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="5100e-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="5100e-163">Representa un marcador para una aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5100e-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="5100e-164">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="5100e-164">Output from the component isn't included.</span></span> <span data-ttu-id="5100e-165">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="5100e-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="5100e-166">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="5100e-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="5100e-167">Mientras que las páginas y las vistas pueden utilizar componentes, lo contrario no es cierto.</span><span class="sxs-lookup"><span data-stu-id="5100e-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="5100e-168">Los componentes no pueden usar escenarios específicos de página y de vista, tales como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="5100e-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="5100e-169">Para usar la lógica de vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="5100e-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="5100e-170">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="5100e-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="5100e-171">Para obtener más información sobre cómo se representan los componentes, el estado del componente y el Asistente de etiquetas `Component`, vea los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5100e-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>