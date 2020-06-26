---
title: Procedimientos recomendados de rendimiento de WebAssembly de Blazor en ASP.NET Core
author: pranavkm
description: Sugerencias para aumentar el rendimiento de las aplicaciones WebAssembly de Blazor en ASP.NET Core y evitar problemas de rendimiento comunes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/08/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/webassembly-performance-best-practices
ms.openlocfilehash: c5169231eec67a43830f761bff7585deff774613
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103490"
---
# <a name="aspnet-core-blazor-webassembly-performance-best-practices"></a><span data-ttu-id="ee29b-103">Procedimientos recomendados de rendimiento de WebAssembly de Blazor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee29b-103">ASP.NET Core Blazor WebAssembly performance best practices</span></span>

<span data-ttu-id="ee29b-104">Por [Pranav Krishnamoorthy](https://github.com/pranavkm)</span><span class="sxs-lookup"><span data-stu-id="ee29b-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm)</span></span>

<span data-ttu-id="ee29b-105">En este artículo se proporcionan instrucciones relativas a procedimientos recomendados de rendimiento de WebAssembly de Blazor en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee29b-105">This article provides guidelines for ASP.NET Core Blazor WebAssembly performance best practices.</span></span>

## <a name="avoid-unnecessary-component-renders"></a><span data-ttu-id="ee29b-106">Evitar representaciones de componentes innecesarias</span><span class="sxs-lookup"><span data-stu-id="ee29b-106">Avoid unnecessary component renders</span></span>

<span data-ttu-id="ee29b-107">El algoritmo de diferenciación de Blazor evita que un componente se represente cuando dicho algoritmo percibe que ese componente no ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="ee29b-107">Blazor's diffing algorithm avoids rerendering a component when the algorithm perceives that the component hasn't changed.</span></span> <span data-ttu-id="ee29b-108">Invalide <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> si prefiere tener un control específico sobre la representación de componentes.</span><span class="sxs-lookup"><span data-stu-id="ee29b-108">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> for fine-grained control over component rendering.</span></span>

<span data-ttu-id="ee29b-109">Si crea un componente de solo interfaz de usuario que nunca cambia tras la representación inicial, configure <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> para que devuelva `false`:</span><span class="sxs-lookup"><span data-stu-id="ee29b-109">If authoring a UI-only component that never changes after the initial render, configure <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to return `false`:</span></span>

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

<span data-ttu-id="ee29b-110">La mayoría de las aplicaciones no requieren un control específico, pero <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> también se puede usar para representar de forma selectiva un componente en respuesta a un evento de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ee29b-110">Most apps don't require fine-grained control, but <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> can also be used to selectively render a component responding to a UI event.</span></span>

<span data-ttu-id="ee29b-111">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee29b-111">In the following example:</span></span>

* <span data-ttu-id="ee29b-112"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> se invalida y se establece en el valor del campo <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>, que es `false` inicialmente, cuando el componente se carga.</span><span class="sxs-lookup"><span data-stu-id="ee29b-112"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is overridden and set to the value of the <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> field, which is initially `false` when the component loads.</span></span>
* <span data-ttu-id="ee29b-113">Cuando el botón se selecciona, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> se establece en `true`, lo que obliga al componente a volver a representarse con el elemento `currentCount` actualizado.</span><span class="sxs-lookup"><span data-stu-id="ee29b-113">When the button is selected, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is set to `true`, which forces the component to rerender with the updated `currentCount`.</span></span>
* <span data-ttu-id="ee29b-114">Inmediatamente después de que el componente se vuelva a representar, <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> establece el valor de <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> de nuevo en `false` para impedir que se vuelva a representar hasta la próxima vez que el botón se seleccione.</span><span class="sxs-lookup"><span data-stu-id="ee29b-114">Immediately after rerendering, <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> sets the value of <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> back to `false` to prevent further rerendering until the next time the button is selected.</span></span>

```razor
<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;
    private bool shouldRender;

    protected override bool ShouldRender() => shouldRender;

    protected override void OnAfterRender(bool first)
    {
        shouldRender = false;
    }

    private void IncrementCount()
    {
        currentCount++;
        shouldRender = true;
    }
}
```

<span data-ttu-id="ee29b-115">Para obtener más información, vea <xref:blazor/components/lifecycle#after-component-render>.</span><span class="sxs-lookup"><span data-stu-id="ee29b-115">For more information, see <xref:blazor/components/lifecycle#after-component-render>.</span></span>

## <a name="virtualize-re-usable-fragments"></a><span data-ttu-id="ee29b-116">Virtualización de fragmentos reutilizables</span><span class="sxs-lookup"><span data-stu-id="ee29b-116">Virtualize re-usable fragments</span></span>

<span data-ttu-id="ee29b-117">Los componentes constituyen un método práctico para generar fragmentos de código y marcado reutilizables.</span><span class="sxs-lookup"><span data-stu-id="ee29b-117">Components offer a convenient approach to produce re-usable fragments of code and markup.</span></span> <span data-ttu-id="ee29b-118">En general, se recomienda crear componentes individuales que se ajusten más adecuadamente a los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee29b-118">In general, we recommend authoring individual components that best align with the app's requirements.</span></span> <span data-ttu-id="ee29b-119">Una advertencia que hacer a este respecto es que cada componente secundario adicional contribuye al tiempo total necesario para representar un componente primario.</span><span class="sxs-lookup"><span data-stu-id="ee29b-119">One caveat is that each additional child component contributes to the total time it takes to render a parent component.</span></span> <span data-ttu-id="ee29b-120">En la mayoría de las aplicaciones, esta sobrecarga adicional es insignificante.</span><span class="sxs-lookup"><span data-stu-id="ee29b-120">For most apps, the additional overhead is negligible.</span></span> <span data-ttu-id="ee29b-121">Las aplicaciones que generan un gran número de componentes deben considerar el uso de estrategias para reducir la sobrecarga de procesamiento, como limitar el número de componentes representados.</span><span class="sxs-lookup"><span data-stu-id="ee29b-121">Apps that produce a large number of components should consider using strategies to reduce processing overhead, such as limiting the number of rendered components.</span></span>

<span data-ttu-id="ee29b-122">Por ejemplo, una cuadrícula o una lista que representa cientos de filas que contienen componentes hace un uso intensivo del procesador para lograr la representación.</span><span class="sxs-lookup"><span data-stu-id="ee29b-122">For example, a grid or list that renders hundreds of rows containing components is processor intensive to render.</span></span> <span data-ttu-id="ee29b-123">Así pues, considere la posibilidad de virtualizar un diseño de lista o cuadrícula para que solo se represente un subconjunto de los componentes en un momento determinado.</span><span class="sxs-lookup"><span data-stu-id="ee29b-123">Consider virtualizing a grid or list layout so that only a subset of the components is rendered at any given time.</span></span> <span data-ttu-id="ee29b-124">Para obtener un ejemplo de representación de un subconjunto de componentes, vea los siguientes componentes en la [aplicación de ejemplo de virtualización (repositorio de GitHub aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):</span><span class="sxs-lookup"><span data-stu-id="ee29b-124">For an example of component subset rendering, see the following components in the [Virtualization sample app (aspnet/samples GitHub repository)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):</span></span>

* <span data-ttu-id="ee29b-125">Componente `Virtualize` ([Shared/virtualizate.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)): componente escrito en C# que implementa <xref:Microsoft.AspNetCore.Components.ComponentBase> para representar un conjunto de filas de datos meteorológicos en función el desplazamiento del usuario con el ratón.</span><span class="sxs-lookup"><span data-stu-id="ee29b-125">`Virtualize` component ([Shared/Virtualize.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)): A component written in C# that implements <xref:Microsoft.AspNetCore.Components.ComponentBase> to render a set of weather data rows based on user scrolling.</span></span>
* <span data-ttu-id="ee29b-126">Componente `FetchData`([Pages/FetchData.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)): usa el componente `Virtualize` para mostrar 25 filas de datos meteorológicos a la vez.</span><span class="sxs-lookup"><span data-stu-id="ee29b-126">`FetchData` component ([Pages/FetchData.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)): Uses the `Virtualize` component to display 25 rows of weather data at a time.</span></span>

## <a name="avoid-javascript-interop-to-marshal-data"></a><span data-ttu-id="ee29b-127">Evitar la interoperabilidad de JavaScript para serializar datos</span><span class="sxs-lookup"><span data-stu-id="ee29b-127">Avoid JavaScript interop to marshal data</span></span>

<span data-ttu-id="ee29b-128">En WebAssembly de Blazor, una llamada de interoperabilidad de JavaScript debe atravesar el límite WebAssembly-JS.</span><span class="sxs-lookup"><span data-stu-id="ee29b-128">In Blazor WebAssembly, a JavaScript (JS) interop call must traverse the WebAssembly-JS boundary.</span></span> <span data-ttu-id="ee29b-129">Al serializar y deserializar contenido en los dos contextos, se crea una sobrecarga de procesamiento para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee29b-129">Serializing and deserializing content across the two contexts creates processing overhead for the app.</span></span> <span data-ttu-id="ee29b-130">Las llamadas de interoperabilidad de JavaScript frecuentes suelen afectar negativamente al rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ee29b-130">Frequent JS interop calls often adversely affects performance.</span></span> <span data-ttu-id="ee29b-131">Para reducir la serialización de datos en el límite, determine si la aplicación es capaz de aglutinar muchas cargas pequeñas en una sola carga grande para evitar el gran volumen de cambios de contexto entre WebAssembly y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee29b-131">To reduce the marshalling of data across the boundary, determine if the app can consolidate many small payloads into a single large payload to avoid the high volume of context switching between WebAssembly and JS.</span></span>

## <a name="use-systemtextjson"></a><span data-ttu-id="ee29b-132">Uso de System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="ee29b-132">Use System.Text.Json</span></span>

<span data-ttu-id="ee29b-133">La implementación de interoperabilidad de JavaScript de Blazor se basa en <xref:System.Text.Json>, que es una biblioteca de serialización de JSON de alto rendimiento con una asignación de memoria reducida.</span><span class="sxs-lookup"><span data-stu-id="ee29b-133">Blazor's JS interop implementation relies on <xref:System.Text.Json>, which is a high-performance JSON serialization library with low memory allocation.</span></span> <span data-ttu-id="ee29b-134">El uso de <xref:System.Text.Json> no da como resultado un mayor tamaño de carga de aplicación frente a la adición de una o varias bibliotecas JSON alternativas.</span><span class="sxs-lookup"><span data-stu-id="ee29b-134">Using <xref:System.Text.Json> doesn't result in additional app payload size over adding one or more alternate JSON libraries.</span></span>

<span data-ttu-id="ee29b-135">Para obtener instrucciones sobre la migración, vea [Migración desde Newtonsoft.json a System.Text.Json](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span><span class="sxs-lookup"><span data-stu-id="ee29b-135">For migration guidance, see [How to migrate from Newtonsoft.Json to System.Text.Json](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span></span>

## <a name="use-synchronous-and-unmarshalled-js-interop-apis-where-appropriate"></a><span data-ttu-id="ee29b-136">Uso de API de interoperabilidad de JavaScript sincrónicas y no serializadas cuando sea necesario</span><span class="sxs-lookup"><span data-stu-id="ee29b-136">Use synchronous and unmarshalled JS interop APIs where appropriate</span></span>

<span data-ttu-id="ee29b-137">WebAssembly de Blazor ofrece dos versiones más de <xref:Microsoft.JSInterop.IJSRuntime> con respecto a la única versión disponible en las aplicaciones Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="ee29b-137">Blazor WebAssembly offers two additional versions of <xref:Microsoft.JSInterop.IJSRuntime> over the single version available to Blazor Server apps:</span></span>

* <span data-ttu-id="ee29b-138"><xref:Microsoft.JSInterop.IJSInProcessRuntime> permite invocar llamadas de interoperabilidad de JavaScript sincrónicamente, lo que genera una sobrecarga menor que las versiones asincrónicas:</span><span class="sxs-lookup"><span data-stu-id="ee29b-138"><xref:Microsoft.JSInterop.IJSInProcessRuntime> allows invoking JS interop calls synchronously, which has less overhead than the asynchronous versions:</span></span>

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (IJSInProcessRuntime)JS;

          var value = jsInProcess.Invoke<string>("jsInteropCall");
      }
  }
  ```

* <span data-ttu-id="ee29b-139"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> permite llamadas de interoperabilidad de JavaScript no serializadas:</span><span class="sxs-lookup"><span data-stu-id="ee29b-139"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> permits unmarshalled JS interop calls:</span></span>

  ```javascript
  function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
  }
  ```

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (WebAssemblyJSRuntime)JS;

          var value = jsInProcess.InvokeUnmarshalled<string>("jsInteropCall");
      }
  }
  ```

  > [!WARNING]
  > <span data-ttu-id="ee29b-140">Aunque el uso de <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> es el método de interoperabilidad de JavaScript con menor sobrecarga, las API de JavaScript necesarias para interactuar con estas API no están documentadas en este momento y están sujetas a cambios importantes en versiones futuras.</span><span class="sxs-lookup"><span data-stu-id="ee29b-140">While using <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> has the least overhead of the JS interop approaches, the JavaScript APIs required to interact with these APIs are currently undocumented and subject to breaking changes in future releases.</span></span>

## <a name="reduce-app-size"></a><span data-ttu-id="ee29b-141">Reducción del tamaño de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ee29b-141">Reduce app size</span></span>

### <a name="intermediate-language-il-linking"></a><span data-ttu-id="ee29b-142">Vinculación de lenguaje intermedio</span><span class="sxs-lookup"><span data-stu-id="ee29b-142">Intermediate Language (IL) linking</span></span>

<span data-ttu-id="ee29b-143">[Cuando una aplicación WebAssembly de Blazor se vincula](xref:blazor/host-and-deploy/configure-linker), el tamaño de la aplicación se reduce recortando el código que no se usa en los archivos binarios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee29b-143">[Linking a Blazor WebAssembly app](xref:blazor/host-and-deploy/configure-linker) reduces the app's size by trimming unused code in the app's binaries.</span></span> <span data-ttu-id="ee29b-144">El vinculador solo está habilitado de forma predeterminada cuando se compila en la configuración `Release`.</span><span class="sxs-lookup"><span data-stu-id="ee29b-144">By default, the linker is only enabled when building in `Release` configuration.</span></span> <span data-ttu-id="ee29b-145">Para sacar partido de esto, publique la aplicación de implementación con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish), con la opción [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) establecida en `Release`:</span><span class="sxs-lookup"><span data-stu-id="ee29b-145">To benefit from this, publish the app for deployment using the [dotnet publish](/dotnet/core/tools/dotnet-publish) command with the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option set to `Release`:</span></span>

```dotnetcli
dotnet publish -c Release
```

### <a name="compression"></a><span data-ttu-id="ee29b-146">Compresión</span><span class="sxs-lookup"><span data-stu-id="ee29b-146">Compression</span></span>

<span data-ttu-id="ee29b-147">Cuando se publica una aplicación de WebAssembly de Blazor, la salida se comprime estáticamente durante la publicación para reducir el tamaño de la aplicación y acabar con la necesidad de compresión en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ee29b-147">When a Blazor WebAssembly app is published, the output is statically compressed during publish to reduce the app's size and remove the overhead for runtime compression.</span></span> Blazor<span data-ttu-id="ee29b-148"> se basa en el servidor para realizar negociación de contenido y proporcionar archivos comprimidos estáticamente.</span><span class="sxs-lookup"><span data-stu-id="ee29b-148"> relies on the server to perform content negotation and serve statically-compressed files.</span></span>

<span data-ttu-id="ee29b-149">Cuando una aplicación se haya implementado, compruebe que proporciona archivos comprimidos.</span><span class="sxs-lookup"><span data-stu-id="ee29b-149">After an app is deployed, verify that the app serves compressed files.</span></span> <span data-ttu-id="ee29b-150">Inspeccione la pestaña Red de las herramientas de desarrollo del explorador y compruebe que los archivos se proporcionan con `Content-Encoding: br` o `Content-Encoding: gz`.</span><span class="sxs-lookup"><span data-stu-id="ee29b-150">Inspect the Network tab in a browser's Developer Tools and verify that the files are served with `Content-Encoding: br` or `Content-Encoding: gz`.</span></span> <span data-ttu-id="ee29b-151">Si el host no proporciona archivos comprimidos, siga las instrucciones de <xref:blazor/host-and-deploy/webassembly#compression>.</span><span class="sxs-lookup"><span data-stu-id="ee29b-151">If the host isn't serving compressed files, follow the instructions in <xref:blazor/host-and-deploy/webassembly#compression>.</span></span>

### <a name="disable-unused-features"></a><span data-ttu-id="ee29b-152">Deshabilitar las características sin usar</span><span class="sxs-lookup"><span data-stu-id="ee29b-152">Disable unused features</span></span>

<span data-ttu-id="ee29b-153">El runtime de WebAssembly de Blazor incluye las siguientes características de .NET, que se pueden deshabilitar si la aplicación no las necesita cuando el tamaño de la carga es más pequeño:</span><span class="sxs-lookup"><span data-stu-id="ee29b-153">Blazor WebAssembly's runtime includes the following .NET features that can be disabled if the app doesn't require them for a smaller payload size:</span></span>

* <span data-ttu-id="ee29b-154">Se incluye un archivo de datos para que la información de zona horaria sea correcta.</span><span class="sxs-lookup"><span data-stu-id="ee29b-154">A data file is included to make timezone information correct.</span></span> <span data-ttu-id="ee29b-155">Si la aplicación no necesita esta característica, considere la posibilidad de deshabilitarla estableciendo en `false` la propiedad de MSBuild `BlazorEnableTimeZoneSupport` del archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ee29b-155">If the app doesn't require this feature, consider disabling it by setting the `BlazorEnableTimeZoneSupport` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

* <span data-ttu-id="ee29b-156">Se incluye información de intercalación para que API como <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="ee29b-156">Collation information is included to make APIs such as <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> work correctly.</span></span> <span data-ttu-id="ee29b-157">Si está convencido de que la aplicación no necesita datos de intercalación, considere la posibilidad de deshabilitarla estableciendo en `false` la propiedad de MSBuild `BlazorWebAssemblyPreserveCollationData` del archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ee29b-157">If you're certain that the app doesn't require the collation data, consider disabling it by setting the `BlazorWebAssemblyPreserveCollationData` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```