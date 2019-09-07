---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/components
ms.openlocfilehash: e877abfb568f71046c3603cac5e888e99ffc8d15
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800423"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="a7d02-103">Crear y usar ASP.NET Core componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="a7d02-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="a7d02-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a7d02-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a7d02-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7d02-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a7d02-106">Las aplicaciones increíbles se compilan con *componentes*.</span><span class="sxs-lookup"><span data-stu-id="a7d02-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="a7d02-107">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="a7d02-108">Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="a7d02-109">Los componentes son flexibles y ligeros.</span><span class="sxs-lookup"><span data-stu-id="a7d02-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="a7d02-110">Se pueden anidar, reutilizar y compartir entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="a7d02-111">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="a7d02-111">Component classes</span></span>

<span data-ttu-id="a7d02-112">Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="a7d02-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="a7d02-113">Un componente de extraordinariamente se conoce como *componente de Razor*.</span><span class="sxs-lookup"><span data-stu-id="a7d02-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="a7d02-114">El nombre de un componente debe empezar con un carácter en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="a7d02-115">Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.</span><span class="sxs-lookup"><span data-stu-id="a7d02-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="a7d02-116">Los componentes se pueden crear con la extensión de archivo *. cshtml* siempre que los archivos se identifiquen como archivos de componentes de `_RazorComponentInclude` Razor mediante la propiedad de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a7d02-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="a7d02-117">Por ejemplo, una aplicación que especifica que todos los archivos *. cshtml* de la carpeta *pages* se deben tratar como archivos de componentes de Razor:</span><span class="sxs-lookup"><span data-stu-id="a7d02-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="a7d02-118">La interfaz de usuario de un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="a7d02-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="a7d02-119">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="a7d02-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a7d02-120">Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="a7d02-121">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="a7d02-122">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a7d02-123">En el `@code` bloque, el estado del componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="a7d02-124">Se permite emplear más de un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="a7d02-125">En las versiones preliminares anteriores de ASP.net Core 3,0 `@functions` , los bloques se usaban para el `@code` mismo propósito que los bloques de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="a7d02-126">`@functions`los bloques continúan funcionando en los componentes de Razor, pero se recomienda `@code` usar el bloque en ASP.net Core 3,0 Preview 6 o posterior.</span><span class="sxs-lookup"><span data-stu-id="a7d02-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="a7d02-127">Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que `@`comienzan por.</span><span class="sxs-lookup"><span data-stu-id="a7d02-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="a7d02-128">Por ejemplo, un C# campo se representa mediante el prefijo `@` del nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="a7d02-129">En el siguiente ejemplo se evalúa y representa:</span><span class="sxs-lookup"><span data-stu-id="a7d02-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="a7d02-130">`_headingFontStyle`al valor de la propiedad CSS `font-style`para.</span><span class="sxs-lookup"><span data-stu-id="a7d02-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="a7d02-131">`_headingText`al contenido del `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="a7d02-132">Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="a7d02-133">Después compara el nuevo árbol de representación con el anterior y aplica las modificaciones realizadas en el Document Object Model del explorador (DOM).</span><span class="sxs-lookup"><span data-stu-id="a7d02-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="a7d02-134">Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="a7d02-135">Los componentes que generan páginas web normalmente residen en la carpeta *pages* .</span><span class="sxs-lookup"><span data-stu-id="a7d02-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="a7d02-136">Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="a7d02-137">Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="a7d02-138">Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando `WebApplication`el espacio de nombres raíz de la aplicación es:</span><span class="sxs-lookup"><span data-stu-id="a7d02-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="a7d02-139">Integración de componentes en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="a7d02-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="a7d02-140">Usar componentes con las aplicaciones de Razor Pages y MVC existentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="a7d02-141">No es necesario volver a escribir las páginas o vistas existentes para utilizar los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="a7d02-142">Cuando se representa la página o la vista, los componentes se preprocesan al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="a7d02-143">Para representar un componente de una página o vista, use el `RenderComponentAsync<TComponent>` método auxiliar HTML:</span><span class="sxs-lookup"><span data-stu-id="a7d02-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="a7d02-144">Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="a7d02-145">Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="a7d02-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="a7d02-146">Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="a7d02-147">Para obtener más información sobre cómo se representan los componentes y cómo se administra el estado del componente en las aplicaciones de servidor más <xref:blazor/hosting-models> increíbles, consulte el artículo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="a7d02-148">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="a7d02-148">Use components</span></span>

<span data-ttu-id="a7d02-149">Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="a7d02-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="a7d02-150">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="a7d02-151">El enlace de atributo distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="a7d02-152">Por ejemplo, `@bind` es válido y `@Bind` no es válido.</span><span class="sxs-lookup"><span data-stu-id="a7d02-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="a7d02-153">El marcado siguiente en *index. Razor* representa una `HeadingComponent` instancia de:</span><span class="sxs-lookup"><span data-stu-id="a7d02-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="a7d02-154">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="a7d02-155">Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="a7d02-156">La adición `@using` de una instrucción para el espacio de nombres del componente hace que el componente esté disponible, lo que elimina la advertencia.</span><span class="sxs-lookup"><span data-stu-id="a7d02-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="a7d02-157">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="a7d02-157">Component parameters</span></span>

<span data-ttu-id="a7d02-158">Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de `[Parameter]` componente con el atributo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="a7d02-159">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="a7d02-160">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="a7d02-161">En el ejemplo siguiente, el `ParentComponent` establece el valor de la `Title` propiedad de `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="a7d02-162">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="a7d02-163">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="a7d02-163">Child content</span></span>

<span data-ttu-id="a7d02-164">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-164">Components can set the content of another component.</span></span> <span data-ttu-id="a7d02-165">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="a7d02-166">En el ejemplo siguiente, `ChildComponent` tiene una `ChildContent` propiedad que representa un `RenderFragment`, que representa un segmento de la interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="a7d02-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="a7d02-167">El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="a7d02-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="a7d02-168">El valor de `ChildContent` se recibe del componente primario y se representa dentro del del panel de `panel-body`arranque.</span><span class="sxs-lookup"><span data-stu-id="a7d02-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="a7d02-169">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="a7d02-170">La propiedad que recibe `RenderFragment` el contenido debe denominarse `ChildContent` por Convención.</span><span class="sxs-lookup"><span data-stu-id="a7d02-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="a7d02-171">Lo siguiente `ParentComponent` puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de `<ChildComponent>` las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="a7d02-172">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="a7d02-173">Atributos expansión y parámetros arbitrarios</span><span class="sxs-lookup"><span data-stu-id="a7d02-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="a7d02-174">Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="a7d02-175">Los atributos adicionales se pueden capturar en un diccionario y, a continuación, *splatted* en un elemento cuando el componente se [@attributes](xref:mvc/views/razor#attributes) representa mediante la Directiva Razor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="a7d02-176">Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones.</span><span class="sxs-lookup"><span data-stu-id="a7d02-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="a7d02-177">Por ejemplo, puede resultar tedioso definir atributos por separado para un `<input>` que admita muchos parámetros.</span><span class="sxs-lookup"><span data-stu-id="a7d02-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="a7d02-178">En el ejemplo siguiente, el primer `<input>` elemento (`id="useIndividualParams"`) usa parámetros de componente individuales, mientras que `<input>` el segundo`id="useAttributesDict"`elemento () utiliza el atributo expansión:</span><span class="sxs-lookup"><span data-stu-id="a7d02-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="a7d02-179">El tipo del parámetro debe implementarse `IEnumerable<KeyValuePair<string, object>>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="a7d02-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="a7d02-180">El `IReadOnlyDictionary<string, object>` uso de también es una opción en este escenario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="a7d02-181">Los `<input>` elementos representados con ambos enfoques son idénticos:</span><span class="sxs-lookup"><span data-stu-id="a7d02-181">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="a7d02-182">Para aceptar atributos arbitrarios, defina un parámetro de componente `[Parameter]` mediante el atributo `CaptureUnmatchedValues` con la propiedad `true`establecida en:</span><span class="sxs-lookup"><span data-stu-id="a7d02-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="a7d02-183">La `CaptureUnmatchedValues` propiedad en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="a7d02-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="a7d02-184">Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="a7d02-185">El tipo de propiedad que `CaptureUnmatchedValues` se usa con debe ser asignable desde con claves de `Dictionary<string, object>` cadena.</span><span class="sxs-lookup"><span data-stu-id="a7d02-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="a7d02-186">`IEnumerable<KeyValuePair<string, object>>`o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="a7d02-187">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="a7d02-187">Data binding</span></span>

<span data-ttu-id="a7d02-188">El enlace de datos tanto a componentes como a elementos DOM se [@bind](xref:mvc/views/razor#bind) realiza con el atributo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="a7d02-189">En el siguiente ejemplo se enlaza `_italicsCheck` el campo al estado activado de la casilla:</span><span class="sxs-lookup"><span data-stu-id="a7d02-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="a7d02-190">Cuando la casilla está activada y desactivada, el valor de la propiedad se `true` actualiza `false`a y, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="a7d02-191">La casilla solo se actualiza en la interfaz de usuario cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7d02-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="a7d02-192">Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades normalmente se reflejan en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="a7d02-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="a7d02-193">Usar `@bind` con una `CurrentValue` propiedad (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="a7d02-194">Cuando se representa el componente, el `value` del elemento de entrada procede de la `CurrentValue` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7d02-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="a7d02-195">Cuando el usuario escribe en el cuadro de texto, `onchange` se desencadena el evento y `CurrentValue` la propiedad se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="a7d02-196">En realidad, la generación de código es un poco más compleja `@bind` porque controla algunos casos en los que se realizan conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="a7d02-197">En principio, `@bind` asocia el valor actual de una expresión a un `value` atributo y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="a7d02-198">`onchange` Además de controlar los eventos con `@bind` sintaxis, una propiedad o un campo se pueden enlazar mediante otros eventos mediante la [@bind-value](xref:mvc/views/razor#bind) especificación de un `event` atributo con[@bind-value:event](xref:mvc/views/razor#bind)un parámetro ().</span><span class="sxs-lookup"><span data-stu-id="a7d02-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="a7d02-199">En el ejemplo siguiente se enlaza `CurrentValue` la propiedad del evento:`oninput`</span><span class="sxs-lookup"><span data-stu-id="a7d02-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="a7d02-200">A diferencia `onchange`de, que se activa cuando el elemento pierde `oninput` el foco, se desencadena cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="a7d02-201">**Globalización**</span><span class="sxs-lookup"><span data-stu-id="a7d02-201">**Globalization**</span></span>

<span data-ttu-id="a7d02-202">`@bind`se da formato a los valores para mostrarlos y analizarlos con las reglas de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="a7d02-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="a7d02-203">Se puede tener acceso a la referencia cultural actual <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> desde la propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7d02-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="a7d02-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo`<input type="{TYPE}" />`():</span><span class="sxs-lookup"><span data-stu-id="a7d02-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="a7d02-205">Los tipos de campo anteriores:</span><span class="sxs-lookup"><span data-stu-id="a7d02-205">The preceding field types:</span></span>

* <span data-ttu-id="a7d02-206">Se muestran con sus reglas de formato basadas en explorador adecuadas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="a7d02-207">No puede contener texto de forma libre.</span><span class="sxs-lookup"><span data-stu-id="a7d02-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="a7d02-208">Proporcionar características de interacción con el usuario en función de la implementación del explorador.</span><span class="sxs-lookup"><span data-stu-id="a7d02-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="a7d02-209">Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten en estos momentos porque no son compatibles con todos los exploradores principales:</span><span class="sxs-lookup"><span data-stu-id="a7d02-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="a7d02-210">`@bind`admite el `@bind:culture` parámetro para proporcionar un <xref:System.Globalization.CultureInfo?displayProperty=fullName> para analizar y dar formato a un valor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="a7d02-211">No se recomienda especificar una referencia cultural al usar `date` los `number` tipos de campo y.</span><span class="sxs-lookup"><span data-stu-id="a7d02-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="a7d02-212">`date`y `number` tienen compatibilidad más ligera que proporciona la referencia cultural necesaria.</span><span class="sxs-lookup"><span data-stu-id="a7d02-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="a7d02-213">Para obtener información sobre cómo establecer la referencia cultural del usuario, consulte la sección [Localización](#localization).</span><span class="sxs-lookup"><span data-stu-id="a7d02-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="a7d02-214">**Cadenas de formato**</span><span class="sxs-lookup"><span data-stu-id="a7d02-214">**Format strings**</span></span>

<span data-ttu-id="a7d02-215">El enlace de datos <xref:System.DateTime> funciona con cadenas [@bind:format](xref:mvc/views/razor#bind)de formato mediante.</span><span class="sxs-lookup"><span data-stu-id="a7d02-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="a7d02-216">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="a7d02-217">En el código anterior, el `<input>` tipo de campo del elemento`type`() tiene como `text`valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="a7d02-218">`@bind:format`se admite para enlazar los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="a7d02-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="a7d02-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a7d02-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="a7d02-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a7d02-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="a7d02-221">El `@bind:format` atributo especifica el formato `value` de fecha que se va a aplicar `<input>` al del elemento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="a7d02-222">El formato también se usa para analizar el valor cuando se `onchange` produce un evento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="a7d02-223">No se recomienda especificar un formato `date` para el tipo de campo porque el increíble tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="a7d02-224">**Parámetros de componente**</span><span class="sxs-lookup"><span data-stu-id="a7d02-224">**Component parameters**</span></span>

<span data-ttu-id="a7d02-225">El enlace reconoce los parámetros de `@bind-{property}` componente, donde puede enlazar un valor de propiedad entre los componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="a7d02-226">El siguiente componente secundario (`ChildComponent`) tiene un `Year` parámetro de componente `YearChanged` y una devolución de llamada:</span><span class="sxs-lookup"><span data-stu-id="a7d02-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="a7d02-227">`EventCallback<T>`se explica en la sección [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="a7d02-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="a7d02-228">El siguiente componente primario utiliza `ChildComponent` y enlaza el `ParentYear` parámetro del elemento primario con el `Year` parámetro del componente secundario:</span><span class="sxs-lookup"><span data-stu-id="a7d02-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="a7d02-229">Al `ParentComponent` cargar, se genera el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="a7d02-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="a7d02-230">Si el valor de la `ParentYear` propiedad se cambia seleccionando el botón `ParentComponent`en `ChildComponent` , se actualiza `Year` la propiedad de.</span><span class="sxs-lookup"><span data-stu-id="a7d02-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="a7d02-231">El nuevo valor de `Year` se representa en la interfaz de usuario cuando `ParentComponent` se rerepresenta el.</span><span class="sxs-lookup"><span data-stu-id="a7d02-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="a7d02-232">El `Year` parámetro es enlazable porque tiene un evento complementario `YearChanged` que `Year` coincide con el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="a7d02-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="a7d02-233">Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="a7d02-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="a7d02-234">En general, una propiedad se puede enlazar a un controlador de eventos `@bind-property:event` correspondiente mediante el atributo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="a7d02-235">Por ejemplo, la propiedad `MyProp` se puede enlazar `MyEventHandler` a mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a7d02-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="a7d02-236">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="a7d02-236">Event handling</span></span>

<span data-ttu-id="a7d02-237">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-237">Razor components provide event handling features.</span></span> <span data-ttu-id="a7d02-238">Para un atributo de elemento HTML `on{event}` denominado (por ejemplo `onclick` , `onsubmit`y) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="a7d02-239">El nombre del atributo siempre tiene el formato [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="a7d02-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="a7d02-240">El código siguiente llama `UpdateHeading` al método cuando se selecciona el botón en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="a7d02-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="a7d02-241">El código siguiente llama `CheckChanged` al método cuando se cambia la casilla en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="a7d02-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="a7d02-242">Los controladores de eventos también pueden ser asincrónicos y devolver <xref:System.Threading.Tasks.Task>un.</span><span class="sxs-lookup"><span data-stu-id="a7d02-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="a7d02-243">No es necesario llamar `StateHasChanged()`a.</span><span class="sxs-lookup"><span data-stu-id="a7d02-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="a7d02-244">Las excepciones se registran cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="a7d02-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="a7d02-245">En el ejemplo siguiente, `UpdateHeading` se llama a de forma asincrónica cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="a7d02-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="a7d02-246">Tipos de argumentos de evento</span><span class="sxs-lookup"><span data-stu-id="a7d02-246">Event argument types</span></span>

<span data-ttu-id="a7d02-247">En algunos eventos, se permiten los tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="a7d02-248">Si no es necesario el acceso a uno de estos tipos de evento, no es necesario en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="a7d02-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="a7d02-249">Los [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) admitidos se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-249">Supported [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) are shown in the following table.</span></span>

| <span data-ttu-id="a7d02-250">Evento</span><span class="sxs-lookup"><span data-stu-id="a7d02-250">Event</span></span> | <span data-ttu-id="a7d02-251">Clase</span><span class="sxs-lookup"><span data-stu-id="a7d02-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="a7d02-252">Portapapeles</span><span class="sxs-lookup"><span data-stu-id="a7d02-252">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="a7d02-253">Arrastre</span><span class="sxs-lookup"><span data-stu-id="a7d02-253">Drag</span></span>             | <span data-ttu-id="a7d02-254">`DragEventArgs`y contienen datos de`DataTransferItem` elementos arrastrados. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="a7d02-254">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="a7d02-255">Error</span><span class="sxs-lookup"><span data-stu-id="a7d02-255">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="a7d02-256">Foco</span><span class="sxs-lookup"><span data-stu-id="a7d02-256">Focus</span></span>            | <span data-ttu-id="a7d02-257">`FocusEventArgs`&ndash; No incluye compatibilidad con `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-257">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="a7d02-258">Cambio de`<input>`</span><span class="sxs-lookup"><span data-stu-id="a7d02-258">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="a7d02-259">Teclado</span><span class="sxs-lookup"><span data-stu-id="a7d02-259">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="a7d02-260">Mouse</span><span class="sxs-lookup"><span data-stu-id="a7d02-260">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="a7d02-261">Puntero del mouse</span><span class="sxs-lookup"><span data-stu-id="a7d02-261">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="a7d02-262">Rueda del mouse</span><span class="sxs-lookup"><span data-stu-id="a7d02-262">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="a7d02-263">Progreso</span><span class="sxs-lookup"><span data-stu-id="a7d02-263">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="a7d02-264">Entrada táctil</span><span class="sxs-lookup"><span data-stu-id="a7d02-264">Touch</span></span>            | <span data-ttu-id="a7d02-265">`TouchEventArgs`&ndash; representaunúnicopuntodecontactoenundispositivo`TouchPoint` con distinción de toque.</span><span class="sxs-lookup"><span data-stu-id="a7d02-265">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="a7d02-266">Para obtener más información sobre las propiedades y el comportamiento de control de eventos de los eventos de la tabla anterior, vea [clases EventArgs en el origen de referencia (versión ASPNET/AspNetCore/3.0-preview9)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="a7d02-266">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="a7d02-267">Expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="a7d02-267">Lambda expressions</span></span>

<span data-ttu-id="a7d02-268">También se pueden usar expresiones lambda:</span><span class="sxs-lookup"><span data-stu-id="a7d02-268">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="a7d02-269">A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-269">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="a7d02-270">En el ejemplo siguiente se crean tres botones, cada uno `UpdateHeading` de los cuales llama a`UIMouseEventArgs`para pasar un argumento de evento`buttonNumber`() y su número de botón () cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="a7d02-270">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="a7d02-271">**No** utilice la variable de bucle (`i`) en un `for` bucle directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="a7d02-271">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="a7d02-272">De lo contrario, todas las expresiones lambda usan la misma `i`variable que hace que el valor de sea el mismo en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="a7d02-272">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="a7d02-273">Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.</span><span class="sxs-lookup"><span data-stu-id="a7d02-273">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="a7d02-274">EventCallback</span><span class="sxs-lookup"><span data-stu-id="a7d02-274">EventCallback</span></span>

<span data-ttu-id="a7d02-275">Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce&mdash;un evento de componente secundario, por ejemplo, cuando se produce un `onclick` evento en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-275">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="a7d02-276">Para exponer eventos entre componentes, use `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-276">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="a7d02-277">Un componente primario puede asignar un método de devolución de llamada a un `EventCallback`elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-277">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="a7d02-278">En la aplicación de ejemplo se muestra cómo se configura `onclick` el controlador de un botón para recibir `EventCallback` un delegado del del `ParentComponent`ejemplo. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="a7d02-278">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="a7d02-279">Se escribe con `UIMouseEventArgs`, que es adecuado para un `onclick` evento desde un dispositivo periférico: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="a7d02-279">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="a7d02-280">Establece el elemento secundario en su `ShowMessage`método: `EventCallback<T>` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="a7d02-280">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="a7d02-281">Cuando el botón está seleccionado en la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="a7d02-281">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="a7d02-282">Se `ParentComponent`llama `ShowMessage` al método de.</span><span class="sxs-lookup"><span data-stu-id="a7d02-282">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="a7d02-283">`messageText`se actualiza y se muestra en `ParentComponent`el.</span><span class="sxs-lookup"><span data-stu-id="a7d02-283">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="a7d02-284">No se requiere `StateHasChanged` una llamada a en el método de la`ShowMessage`devolución de llamada ().</span><span class="sxs-lookup"><span data-stu-id="a7d02-284">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="a7d02-285">`StateHasChanged`se llama automáticamente a para representarlo, del mismo modo que los eventos secundarios desencadenan la `ParentComponent`rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-285">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="a7d02-286">`EventCallback`y `EventCallback<T>` permiten los delegados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-286">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="a7d02-287">`EventCallback<T>`está fuertemente tipado y requiere un tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="a7d02-287">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="a7d02-288">`EventCallback`está débilmente tipado y permite cualquier tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-288">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="a7d02-289">Invoca un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y espera el <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="a7d02-289">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="a7d02-290">Use `EventCallback` y`EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.</span><span class="sxs-lookup"><span data-stu-id="a7d02-290">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="a7d02-291">Prefiera el fuertemente tipado `EventCallback<T>`. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="a7d02-291">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="a7d02-292">`EventCallback<T>`proporciona mejores comentarios de errores a los usuarios del componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-292">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="a7d02-293">Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional.</span><span class="sxs-lookup"><span data-stu-id="a7d02-293">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="a7d02-294">Se `EventCallback` usa cuando no hay ningún valor pasado a la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="a7d02-294">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="a7d02-295">Capturar referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="a7d02-295">Capture references to components</span></span>

<span data-ttu-id="a7d02-296">Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa `Show` instancia `Reset`, como o.</span><span class="sxs-lookup"><span data-stu-id="a7d02-296">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="a7d02-297">Para capturar una referencia de componente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-297">To capture a component reference:</span></span>

* <span data-ttu-id="a7d02-298">Agregue un [@ref](xref:mvc/views/razor#ref) atributo al componente secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-298">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="a7d02-299">Defina un campo con el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-299">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="a7d02-300">Cuando se representa el componente, el `loginDialog` campo se rellena con la instancia del `MyLoginDialog` componente secundario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-300">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="a7d02-301">Después, puede invocar métodos .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-301">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7d02-302">La `loginDialog` variable solo se rellena después de que el componente se represente y su salida `MyLoginDialog` incluye el elemento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-302">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="a7d02-303">Hasta ese momento, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="a7d02-303">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="a7d02-304">Para manipular las referencias de componentes una vez finalizada la representación del componente, `OnAfterRenderAsync` use `OnAfterRender` los métodos o.</span><span class="sxs-lookup"><span data-stu-id="a7d02-304">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="a7d02-305">Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="a7d02-305">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="a7d02-306">Las referencias de componente no se pasan&mdash;al código JavaScript solo se usan en código .net.</span><span class="sxs-lookup"><span data-stu-id="a7d02-306">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="a7d02-307">**No** utilice referencias de componentes para mutar el estado de los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="a7d02-307">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="a7d02-308">En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="a7d02-308">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="a7d02-309">El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-309">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="a7d02-310">Invocar métodos de componentes externamente para actualizar el estado</span><span class="sxs-lookup"><span data-stu-id="a7d02-310">Invoke component methods externally to update state</span></span>

<span data-ttu-id="a7d02-311">Increíble utiliza `SynchronizationContext` para exigir un único subproceso lógico de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a7d02-311">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="a7d02-312">Los métodos de ciclo de vida de un componente y todas las devoluciones de llamada de eventos que `SynchronizationContext`se producen con el método increíblemente se ejecutan en este.</span><span class="sxs-lookup"><span data-stu-id="a7d02-312">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="a7d02-313">En el caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, `InvokeAsync` use el método, que se enviará a `SynchronizationContext`la más extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="a7d02-313">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="a7d02-314">Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:</span><span class="sxs-lookup"><span data-stu-id="a7d02-314">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Action<string, int, Task> Notify;
}
```

<span data-ttu-id="a7d02-315">Uso de `NotifierService` para actualizar un componente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-315">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="a7d02-316">En el ejemplo anterior, `NotifierService` invoca el método del `OnNotify` componente `SynchronizationContext`fuera de la extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="a7d02-316">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="a7d02-317">`InvokeAsync`se utiliza para cambiar al contexto correcto y poner en cola una representación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-317">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="a7d02-318">Usar \@la clave para controlar la preservación de elementos y componentes</span><span class="sxs-lookup"><span data-stu-id="a7d02-318">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="a7d02-319">Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de diferenciación de más increíble debe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-319">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="a7d02-320">Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.</span><span class="sxs-lookup"><span data-stu-id="a7d02-320">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="a7d02-321">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-321">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="a7d02-322">El contenido de la `People` colección puede cambiar con entradas insertadas, eliminadas o reordenadas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-322">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="a7d02-323">Cuando el componente se representa, el `<DetailsEditor>` componente puede cambiar para recibir distintos `Details` valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="a7d02-323">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="a7d02-324">Esto puede producir una rerepresentación más compleja de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-324">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="a7d02-325">En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="a7d02-325">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="a7d02-326">El proceso de asignación se puede controlar con `@key` el atributo de directiva.</span><span class="sxs-lookup"><span data-stu-id="a7d02-326">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="a7d02-327">`@key`hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:</span><span class="sxs-lookup"><span data-stu-id="a7d02-327">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="a7d02-328">Cuando la `People` colección cambia, el algoritmo de comparación mantiene la asociación entre `<DetailsEditor>` instancias e `person` instancias:</span><span class="sxs-lookup"><span data-stu-id="a7d02-328">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="a7d02-329">Si se `Person` elimina un de la `People` lista, solo se quita `<DetailsEditor>` la instancia correspondiente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-329">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="a7d02-330">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="a7d02-330">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a7d02-331">Si se `Person` inserta un en alguna posición de la lista, se inserta `<DetailsEditor>` una nueva instancia en la posición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-331">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="a7d02-332">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="a7d02-332">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a7d02-333">Si `Person` se vuelven a ordenar las entradas, las `<DetailsEditor>` instancias correspondientes se conservan y se vuelven a ordenar en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-333">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="a7d02-334">En algunos escenarios, el uso `@key` de reduce la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.</span><span class="sxs-lookup"><span data-stu-id="a7d02-334">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7d02-335">Las claves son locales para cada elemento contenedor o componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-335">Keys are local to each container element or component.</span></span> <span data-ttu-id="a7d02-336">Las claves no se comparan globalmente en todo el documento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-336">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="a7d02-337">Cuándo usar \@la clave</span><span class="sxs-lookup"><span data-stu-id="a7d02-337">When to use \@key</span></span>

<span data-ttu-id="a7d02-338">Normalmente, tiene sentido usar `@key` siempre que se represente una lista (por ejemplo, en un `@foreach` bloque) y exista un `@key`valor adecuado para definir.</span><span class="sxs-lookup"><span data-stu-id="a7d02-338">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="a7d02-339">También puede usar `@key` para evitar que un elemento o subárbol de componentes se conserve cuando un objeto cambia:</span><span class="sxs-lookup"><span data-stu-id="a7d02-339">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="a7d02-340">Si `@currentPerson` cambia, la `@key` Directiva de atributo fuerza a increíblemente a descartar todo `<div>` y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-340">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="a7d02-341">Esto puede ser útil si necesita asegurarse de que no se conserva ningún estado de la `@currentPerson` interfaz de usuario cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="a7d02-341">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="a7d02-342">Cuando no se use \@la clave</span><span class="sxs-lookup"><span data-stu-id="a7d02-342">When not to use \@key</span></span>

<span data-ttu-id="a7d02-343">Existe un costo de rendimiento al diferenciar con `@key`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-343">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="a7d02-344">El costo de rendimiento no es grande, pero `@key` solo se especifica si el control de las reglas de conservación de elementos o componentes beneficia a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-344">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="a7d02-345">Incluso si `@key` no se usa, increíblementeer conserva las instancias de componentes y elementos secundarios tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="a7d02-345">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="a7d02-346">La única ventaja de usar `@key` es controlar el *modo* en que se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de comparación que selecciona la asignación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-346">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="a7d02-347">Qué valores se van a \@usar para Key</span><span class="sxs-lookup"><span data-stu-id="a7d02-347">What values to use for \@key</span></span>

<span data-ttu-id="a7d02-348">Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="a7d02-348">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="a7d02-349">Instancias de objeto de modelo (por ejemplo `Person` , una instancia como en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="a7d02-349">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="a7d02-350">Esto garantiza la conservación en función de la igualdad de la referencia de objeto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-350">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="a7d02-351">Los identificadores únicos (por ejemplo, los valores de clave `int`principal `string`de tipo `Guid`, o).</span><span class="sxs-lookup"><span data-stu-id="a7d02-351">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="a7d02-352">Asegúrese de que los valores `@key` usados para no entren en conflicto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-352">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="a7d02-353">Si se detectan valores en conflicto en el mismo elemento primario, el valor de la excepción extraordinaria produce una excepción porque no puede asignar de forma determinista los elementos o componentes antiguos a los nuevos elementos o componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-353">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="a7d02-354">Utilice solo valores distintos, como instancias de objeto o valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="a7d02-354">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="a7d02-355">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="a7d02-355">Lifecycle methods</span></span>

<span data-ttu-id="a7d02-356">`OnInitializedAsync`y `OnInitialized` ejecute el código para inicializar el componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-356">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="a7d02-357">Para realizar una operación asincrónica, use `OnInitializedAsync` y la `await` palabra clave en la operación:</span><span class="sxs-lookup"><span data-stu-id="a7d02-357">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="a7d02-358">Para una operación sincrónica, use `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="a7d02-358">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="a7d02-359">`OnParametersSetAsync`se `OnParametersSet` llama a y cuando un componente ha recibido parámetros de su elemento primario y los valores se asignan a propiedades.</span><span class="sxs-lookup"><span data-stu-id="a7d02-359">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="a7d02-360">Estos métodos se ejecutan después de la inicialización del componente y cada vez que se representa el componente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-360">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="a7d02-361">`OnAfterRenderAsync`se `OnAfterRender` llama a y después de que un componente ha terminado la representación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-361">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="a7d02-362">En este punto se rellenan las referencias a elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-362">Element and component references are populated at this point.</span></span> <span data-ttu-id="a7d02-363">Use esta fase para realizar pasos de inicialización adicionales mediante el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.</span><span class="sxs-lookup"><span data-stu-id="a7d02-363">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="a7d02-364">`OnAfterRender`*no se llama a cuando se preprocesa en el servidor.*</span><span class="sxs-lookup"><span data-stu-id="a7d02-364">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="a7d02-365">El `firstRender` parámetro para `OnAfterRenderAsync` y `OnAfterRender` es:</span><span class="sxs-lookup"><span data-stu-id="a7d02-365">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="a7d02-366">Se establece `true` en la primera vez que se invoca la instancia de componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-366">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="a7d02-367">Garantiza que el trabajo de inicialización solo se realiza una vez.</span><span class="sxs-lookup"><span data-stu-id="a7d02-367">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="a7d02-368">Controlar acciones asincrónicas incompletas en la representación</span><span class="sxs-lookup"><span data-stu-id="a7d02-368">Handle incomplete async actions at render</span></span>

<span data-ttu-id="a7d02-369">Es posible que las acciones asincrónicas realizadas en eventos del ciclo de vida no se hayan completado antes de que se represente el componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-369">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="a7d02-370">Los objetos se `null` pueden rellenar o completar con datos mientras se ejecuta el método de ciclo de vida.</span><span class="sxs-lookup"><span data-stu-id="a7d02-370">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="a7d02-371">Proporcione lógica de representación para confirmar que los objetos se inicializan.</span><span class="sxs-lookup"><span data-stu-id="a7d02-371">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="a7d02-372">Representar los elementos de la interfaz de usuario del marcador de posición (por ejemplo `null`, un mensaje de carga) mientras los objetos son.</span><span class="sxs-lookup"><span data-stu-id="a7d02-372">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="a7d02-373">En el `FetchData` componente de las plantillas extraordinarias, `OnInitializedAsync` se reemplaza a asincrónica recibir datos de previsión (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="a7d02-373">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="a7d02-374">Cuando `forecasts` es`null`, se muestra al usuario un mensaje de carga.</span><span class="sxs-lookup"><span data-stu-id="a7d02-374">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="a7d02-375">Después de `Task` que `OnInitializedAsync` finalice, el componente se representará con el estado actualizado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-375">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="a7d02-376">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-376">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="a7d02-377">Ejecutar código antes de establecer los parámetros</span><span class="sxs-lookup"><span data-stu-id="a7d02-377">Execute code before parameters are set</span></span>

<span data-ttu-id="a7d02-378">`SetParameters`se puede invalidar para ejecutar el código antes de que se establezcan los parámetros:</span><span class="sxs-lookup"><span data-stu-id="a7d02-378">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="a7d02-379">Si `base.SetParameters` no se invoca, el código personalizado puede interpretar el valor de los parámetros entrantes de cualquier manera que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-379">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="a7d02-380">Por ejemplo, no es necesario asignar los parámetros de entrada a las propiedades de la clase.</span><span class="sxs-lookup"><span data-stu-id="a7d02-380">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="a7d02-381">Suprimir la actualización de la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="a7d02-381">Suppress refreshing of the UI</span></span>

<span data-ttu-id="a7d02-382">`ShouldRender`se puede invalidar para suprimir la actualización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-382">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="a7d02-383">Si se devuelve `true`la implementación, se actualiza la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-383">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="a7d02-384">Incluso si `ShouldRender` se reemplaza, el componente siempre se representa inicialmente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-384">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="a7d02-385">Eliminación de componentes con IDisposable</span><span class="sxs-lookup"><span data-stu-id="a7d02-385">Component disposal with IDisposable</span></span>

<span data-ttu-id="a7d02-386">Si un componente implementa <xref:System.IDisposable>, se llama al [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) cuando se quita el componente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-386">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="a7d02-387">El siguiente componente utiliza `@implements IDisposable` y el `Dispose` método:</span><span class="sxs-lookup"><span data-stu-id="a7d02-387">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="a7d02-388">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="a7d02-388">Routing</span></span>

<span data-ttu-id="a7d02-389">El enrutamiento en extraordinaria se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-389">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="a7d02-390">Cuando se compila un archivo de `@page` Razor con una directiva, a la clase generada <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> se le asigna un que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a7d02-390">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="a7d02-391">En tiempo de ejecución, el enrutador busca clases `RouteAttribute` de componentes con un y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="a7d02-391">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="a7d02-392">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-392">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a7d02-393">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="a7d02-393">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="a7d02-394">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="a7d02-394">Route parameters</span></span>

<span data-ttu-id="a7d02-395">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta `@page` proporcionada en la Directiva.</span><span class="sxs-lookup"><span data-stu-id="a7d02-395">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="a7d02-396">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-396">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="a7d02-397">*Componente de parámetro de ruta*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-397">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="a7d02-398">Los parámetros opcionales no se admiten `@page` , por lo que se aplican dos directivas en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="a7d02-398">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="a7d02-399">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="a7d02-399">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a7d02-400">La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7d02-400">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="a7d02-401">Herencia de clase base para una experiencia de "código subyacente"</span><span class="sxs-lookup"><span data-stu-id="a7d02-401">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="a7d02-402">Los archivos de componentes mezclan C# el marcado HTML y el código de procesamiento en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-402">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="a7d02-403">La `@inherits` Directiva se puede usar para proporcionar aplicaciones increíbles con una experiencia de "código subyacente" que separa el marcado de componentes del código de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="a7d02-403">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="a7d02-404">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) muestra cómo un componente puede heredar una clase `BlazorRocksBase`base,, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-404">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="a7d02-405">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-405">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="a7d02-406">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-406">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="a7d02-407">La clase base debe derivar `ComponentBase`de.</span><span class="sxs-lookup"><span data-stu-id="a7d02-407">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="a7d02-408">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="a7d02-408">Import components</span></span>

<span data-ttu-id="a7d02-409">El espacio de nombres de un componente creado con Razor se basa en:</span><span class="sxs-lookup"><span data-stu-id="a7d02-409">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="a7d02-410">Del `RootNamespace`proyecto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-410">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="a7d02-411">La ruta de acceso desde la raíz del proyecto al componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-411">The path from the project root to the component.</span></span> <span data-ttu-id="a7d02-412">Por ejemplo, `ComponentsSample/Pages/Index.razor` se encuentra en el `ComponentsSample.Pages`espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="a7d02-412">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="a7d02-413">Los componentes C# siguen las reglas de enlace de nombres.</span><span class="sxs-lookup"><span data-stu-id="a7d02-413">Components follow C# name binding rules.</span></span> <span data-ttu-id="a7d02-414">En el caso de *index. Razor*, todos los componentes de la misma carpeta, *páginas*y la carpeta principal, *ComponentsSample*, se encuentran en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="a7d02-414">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="a7d02-415">Los componentes definidos en un espacio de nombres diferente se pueden incluir en el ámbito mediante la directiva [ \@Using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-415">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="a7d02-416">Si existe otro componente `NavMenu.razor`,, en la carpeta `ComponentsSample/Shared/`, el componente se puede utilizar en `Index.razor` con la siguiente `@using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="a7d02-416">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="a7d02-417">También se puede hacer referencia a los componentes mediante sus nombres completos, lo que elimina la necesidad [ \@](xref:mvc/views/razor#using) de la directiva using:</span><span class="sxs-lookup"><span data-stu-id="a7d02-417">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="a7d02-418">No `global::` se admite la calificación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-418">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="a7d02-419">No se admite la importación `using` de componentes con instrucciones con `@using Foo = Bar`alias (por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="a7d02-419">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="a7d02-420">No se admiten nombres parcialmente completos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-420">Partially qualified names aren't supported.</span></span> <span data-ttu-id="a7d02-421">Por ejemplo, no `@using ComponentsSample` se admite `NavMenu.razor` agregar `<Shared.NavMenu></Shared.NavMenu>` y hacer referencia a.</span><span class="sxs-lookup"><span data-stu-id="a7d02-421">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="a7d02-422">Atributos de elementos HTML condicionales</span><span class="sxs-lookup"><span data-stu-id="a7d02-422">Conditional HTML element attributes</span></span>

<span data-ttu-id="a7d02-423">Los atributos de elemento HTML se representan condicionalmente según el valor de .NET.</span><span class="sxs-lookup"><span data-stu-id="a7d02-423">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="a7d02-424">Si el valor es `false` o `null`, el atributo no se representa.</span><span class="sxs-lookup"><span data-stu-id="a7d02-424">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="a7d02-425">Si el valor es `true`, el atributo se representa minimizado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-425">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="a7d02-426">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:</span><span class="sxs-lookup"><span data-stu-id="a7d02-426">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="a7d02-427">Si `IsCompleted` es`true`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="a7d02-427">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="a7d02-428">Si `IsCompleted` es`false`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="a7d02-428">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="a7d02-429">Para obtener más información, consulta <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="a7d02-429">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="a7d02-430">HTML sin formato</span><span class="sxs-lookup"><span data-stu-id="a7d02-430">Raw HTML</span></span>

<span data-ttu-id="a7d02-431">Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal.</span><span class="sxs-lookup"><span data-stu-id="a7d02-431">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="a7d02-432">Para representar HTML sin formato, ajuste el contenido HTML en `MarkupString` un valor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-432">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="a7d02-433">El valor se analiza como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="a7d02-433">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="a7d02-434">La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="a7d02-434">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="a7d02-435">En el ejemplo siguiente se muestra `MarkupString` cómo usar el tipo para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-435">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="a7d02-436">Componentes con plantilla</span><span class="sxs-lookup"><span data-stu-id="a7d02-436">Templated components</span></span>

<span data-ttu-id="a7d02-437">Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-437">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="a7d02-438">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="a7d02-438">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="a7d02-439">Algunos ejemplos son:</span><span class="sxs-lookup"><span data-stu-id="a7d02-439">A couple of examples include:</span></span>

* <span data-ttu-id="a7d02-440">Componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.</span><span class="sxs-lookup"><span data-stu-id="a7d02-440">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="a7d02-441">Componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="a7d02-441">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="a7d02-442">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="a7d02-442">Template parameters</span></span>

<span data-ttu-id="a7d02-443">Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-443">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="a7d02-444">Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="a7d02-444">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="a7d02-445">`RenderFragment<T>`toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-445">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="a7d02-446">`TableTemplate`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-446">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="a7d02-447">Al utilizar un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando elementos secundarios que coincidan con los nombres de`TableHeader` los `RowTemplate` parámetros (y en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="a7d02-447">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="a7d02-448">Parámetros de contexto de plantilla</span><span class="sxs-lookup"><span data-stu-id="a7d02-448">Template context parameters</span></span>

<span data-ttu-id="a7d02-449">Los argumentos de componente `RenderFragment<T>` de tipo pasados como elementos tienen un parámetro `context` implícito denominado (por ejemplo, en el ejemplo `@context.PetId`de código anterior,), pero puede cambiar el nombre `Context` del parámetro mediante el atributo en el elemento secundario. Element.</span><span class="sxs-lookup"><span data-stu-id="a7d02-449">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="a7d02-450">En el ejemplo siguiente, el `RowTemplate` atributo del `Context` elemento especifica el `pet` parámetro:</span><span class="sxs-lookup"><span data-stu-id="a7d02-450">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="a7d02-451">También puede especificar el `Context` atributo en el elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-451">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="a7d02-452">El atributo `Context` especificado se aplica a todos los parámetros de plantilla especificados.</span><span class="sxs-lookup"><span data-stu-id="a7d02-452">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="a7d02-453">Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste).</span><span class="sxs-lookup"><span data-stu-id="a7d02-453">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="a7d02-454">En el ejemplo siguiente, el `Context` atributo aparece en el `TableTemplate` elemento y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="a7d02-454">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="a7d02-455">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="a7d02-455">Generic-typed components</span></span>

<span data-ttu-id="a7d02-456">Los componentes con plantilla suelen tener tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-456">Templated components are often generically typed.</span></span> <span data-ttu-id="a7d02-457">Por ejemplo, se puede `ListViewTemplate` usar un componente genérico para representar `IEnumerable<T>` valores.</span><span class="sxs-lookup"><span data-stu-id="a7d02-457">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="a7d02-458">Para definir un componente genérico, utilice la `@typeparam` Directiva para especificar parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="a7d02-458">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="a7d02-459">Cuando se usan componentes de tipo genérico, el parámetro de tipo se infiere si es posible:</span><span class="sxs-lookup"><span data-stu-id="a7d02-459">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="a7d02-460">De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-460">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="a7d02-461">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="a7d02-461">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="a7d02-462">Valores y parámetros en cascada</span><span class="sxs-lookup"><span data-stu-id="a7d02-462">Cascading values and parameters</span></span>

<span data-ttu-id="a7d02-463">En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-463">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="a7d02-464">Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-464">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="a7d02-465">Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.</span><span class="sxs-lookup"><span data-stu-id="a7d02-465">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="a7d02-466">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="a7d02-466">Theme example</span></span>

<span data-ttu-id="a7d02-467">En el ejemplo siguiente de la aplicación de ejemplo, `ThemeInfo` la clase especifica la información del tema para fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-467">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="a7d02-468">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7d02-468">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="a7d02-469">Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="a7d02-469">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="a7d02-470">El `CascadingValue` componente ajusta un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="a7d02-470">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="a7d02-471">Por ejemplo, la aplicación de ejemplo especifica información de`ThemeInfo`tema () en uno de los diseños de la aplicación como un parámetro en cascada para todos los componentes que componen el `@Body` cuerpo del diseño de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7d02-471">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="a7d02-472">`ButtonClass`se le asigna un valor `btn-success` de en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="a7d02-472">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="a7d02-473">Cualquier componente descendiente puede consumir esta propiedad a través `ThemeInfo` del objeto en cascada.</span><span class="sxs-lookup"><span data-stu-id="a7d02-473">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="a7d02-474">`CascadingValuesParametersLayout`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-474">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="a7d02-475">Para usar valores en cascada, los componentes declaran parámetros en cascada mediante `[CascadingParameter]` el atributo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-475">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="a7d02-476">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-476">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="a7d02-477">En la aplicación de ejemplo, `CascadingValuesParametersTheme` el componente enlaza el `ThemeInfo` valor en cascada a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="a7d02-477">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="a7d02-478">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-478">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="a7d02-479">`CascadingValuesParametersTheme`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-479">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="a7d02-480">Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena `Name` única para cada `CascadingValue` componente y su correspondiente `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-480">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="a7d02-481">En el ejemplo siguiente, dos `CascadingValue` componentes colocan en cascada `MyCascadingType` diferentes instancias de por nombre:</span><span class="sxs-lookup"><span data-stu-id="a7d02-481">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="a7d02-482">En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:</span><span class="sxs-lookup"><span data-stu-id="a7d02-482">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="a7d02-483">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="a7d02-483">TabSet example</span></span>

<span data-ttu-id="a7d02-484">Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-484">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="a7d02-485">Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-485">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="a7d02-486">La aplicación de ejemplo tiene `ITab` una interfaz que las pestañas implementan:</span><span class="sxs-lookup"><span data-stu-id="a7d02-486">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="a7d02-487">El `CascadingValuesParametersTabSet` componente usa el `TabSet` componente, que contiene varios `Tab` componentes:</span><span class="sxs-lookup"><span data-stu-id="a7d02-487">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="a7d02-488">Los componentes `Tab` secundarios no se pasan explícitamente como parámetros `TabSet`a.</span><span class="sxs-lookup"><span data-stu-id="a7d02-488">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="a7d02-489">En su lugar, los `Tab` componentes secundarios forman parte del contenido secundario `TabSet`de.</span><span class="sxs-lookup"><span data-stu-id="a7d02-489">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="a7d02-490">Sin embargo, `TabSet` todavía necesita conocer cada `Tab` componente para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin requerir código adicional, el `TabSet` componente *puede proporcionarse como un valor en cascada* que los componentes descendientes `Tab` recogen.</span><span class="sxs-lookup"><span data-stu-id="a7d02-490">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="a7d02-491">`TabSet`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-491">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="a7d02-492">Los componentes `Tab` descendientes capturan el `TabSet` que contiene como parámetro en cascada, por `Tab` lo que los componentes se `TabSet` agregan a la coordenada y en la que la pestaña está activa.</span><span class="sxs-lookup"><span data-stu-id="a7d02-492">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="a7d02-493">`Tab`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-493">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="a7d02-494">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="a7d02-494">Razor templates</span></span>

<span data-ttu-id="a7d02-495">Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-495">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="a7d02-496">Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="a7d02-496">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="a7d02-497">En el ejemplo siguiente se muestra cómo especificar `RenderFragment` los `RenderFragment<T>` valores y y representar plantillas directamente en un componente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-497">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="a7d02-498">Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="a7d02-498">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="a7d02-499">Salida representada del código anterior:</span><span class="sxs-lookup"><span data-stu-id="a7d02-499">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="a7d02-500">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="a7d02-500">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="a7d02-501">`Microsoft.AspNetCore.Components.RenderTree`proporciona métodos para manipular componentes y elementos, incluida la compilación manual de componentes C# en el código.</span><span class="sxs-lookup"><span data-stu-id="a7d02-501">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="a7d02-502">El uso `RenderTreeBuilder` de para crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-502">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="a7d02-503">Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="a7d02-503">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="a7d02-504">Considere el siguiente `PetDetails` componente, que se puede integrar manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-504">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="a7d02-505">En el ejemplo siguiente, el bucle `CreateComponent` del método genera tres `PetDetails` componentes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-505">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="a7d02-506">Cuando se `RenderTreeBuilder` llama a métodos para crear los`OpenComponent` componentes `AddAttribute`(y), los números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-506">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="a7d02-507">El algoritmo de diferencia más increíble se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-507">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="a7d02-508">Al crear un componente con `RenderTreeBuilder` métodos, codifique los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="a7d02-508">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="a7d02-509">**El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.**</span><span class="sxs-lookup"><span data-stu-id="a7d02-509">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="a7d02-510">Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="a7d02-510">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="a7d02-511">`BuiltContent`pone</span><span class="sxs-lookup"><span data-stu-id="a7d02-511">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="a7d02-512">Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="a7d02-512">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="a7d02-513">Siempre se compilan los archivos extraordinariamente. `.razor`</span><span class="sxs-lookup"><span data-stu-id="a7d02-513">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="a7d02-514">Esta es potencialmente una gran ventaja para `.razor` porque el paso de compilación se puede usar para insertar información que mejoran el rendimiento de las aplicaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a7d02-514">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="a7d02-515">Un ejemplo clave de estas mejoras implican *los números de secuencia*.</span><span class="sxs-lookup"><span data-stu-id="a7d02-515">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="a7d02-516">Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas.</span><span class="sxs-lookup"><span data-stu-id="a7d02-516">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="a7d02-517">El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.</span><span class="sxs-lookup"><span data-stu-id="a7d02-517">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="a7d02-518">Considere el siguiente archivo `.razor` simple:</span><span class="sxs-lookup"><span data-stu-id="a7d02-518">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="a7d02-519">El código anterior se compila en algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7d02-519">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="a7d02-520">Cuando el código se ejecuta por primera vez, si `someFlag` es `true`, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="a7d02-520">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="a7d02-521">Secuencia</span><span class="sxs-lookup"><span data-stu-id="a7d02-521">Sequence</span></span> | <span data-ttu-id="a7d02-522">Type</span><span class="sxs-lookup"><span data-stu-id="a7d02-522">Type</span></span>      | <span data-ttu-id="a7d02-523">Datos</span><span class="sxs-lookup"><span data-stu-id="a7d02-523">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a7d02-524">0</span><span class="sxs-lookup"><span data-stu-id="a7d02-524">0</span></span>        | <span data-ttu-id="a7d02-525">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-525">Text node</span></span> | <span data-ttu-id="a7d02-526">Primero</span><span class="sxs-lookup"><span data-stu-id="a7d02-526">First</span></span>  |
| <span data-ttu-id="a7d02-527">1</span><span class="sxs-lookup"><span data-stu-id="a7d02-527">1</span></span>        | <span data-ttu-id="a7d02-528">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-528">Text node</span></span> | <span data-ttu-id="a7d02-529">Second</span><span class="sxs-lookup"><span data-stu-id="a7d02-529">Second</span></span> |

<span data-ttu-id="a7d02-530">Imagine que `someFlag` se `false`convierte en y que el marcado se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-530">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="a7d02-531">Esta vez, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="a7d02-531">This time, the builder receives:</span></span>

| <span data-ttu-id="a7d02-532">Secuencia</span><span class="sxs-lookup"><span data-stu-id="a7d02-532">Sequence</span></span> | <span data-ttu-id="a7d02-533">Type</span><span class="sxs-lookup"><span data-stu-id="a7d02-533">Type</span></span>       | <span data-ttu-id="a7d02-534">Datos</span><span class="sxs-lookup"><span data-stu-id="a7d02-534">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="a7d02-535">1</span><span class="sxs-lookup"><span data-stu-id="a7d02-535">1</span></span>        | <span data-ttu-id="a7d02-536">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-536">Text node</span></span>  | <span data-ttu-id="a7d02-537">Second</span><span class="sxs-lookup"><span data-stu-id="a7d02-537">Second</span></span> |

<span data-ttu-id="a7d02-538">Cuando el tiempo de ejecución realiza una comparación, observa que se quitó `0` el elemento en la secuencia, por lo que genera el siguiente *script de edición*trivial:</span><span class="sxs-lookup"><span data-stu-id="a7d02-538">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="a7d02-539">Quitar el primer nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-539">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="a7d02-540">Qué sucede si se generan números de secuencia mediante programación</span><span class="sxs-lookup"><span data-stu-id="a7d02-540">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="a7d02-541">Imagine que escribió la siguiente lógica del generador de árboles de representación:</span><span class="sxs-lookup"><span data-stu-id="a7d02-541">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="a7d02-542">Ahora, el primer resultado es:</span><span class="sxs-lookup"><span data-stu-id="a7d02-542">Now, the first output is:</span></span>

| <span data-ttu-id="a7d02-543">Secuencia</span><span class="sxs-lookup"><span data-stu-id="a7d02-543">Sequence</span></span> | <span data-ttu-id="a7d02-544">Type</span><span class="sxs-lookup"><span data-stu-id="a7d02-544">Type</span></span>      | <span data-ttu-id="a7d02-545">Datos</span><span class="sxs-lookup"><span data-stu-id="a7d02-545">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a7d02-546">0</span><span class="sxs-lookup"><span data-stu-id="a7d02-546">0</span></span>        | <span data-ttu-id="a7d02-547">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-547">Text node</span></span> | <span data-ttu-id="a7d02-548">Primero</span><span class="sxs-lookup"><span data-stu-id="a7d02-548">First</span></span>  |
| <span data-ttu-id="a7d02-549">1</span><span class="sxs-lookup"><span data-stu-id="a7d02-549">1</span></span>        | <span data-ttu-id="a7d02-550">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-550">Text node</span></span> | <span data-ttu-id="a7d02-551">Second</span><span class="sxs-lookup"><span data-stu-id="a7d02-551">Second</span></span> |

<span data-ttu-id="a7d02-552">Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo.</span><span class="sxs-lookup"><span data-stu-id="a7d02-552">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="a7d02-553">`someFlag`está `false` en la segunda representación y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="a7d02-553">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="a7d02-554">Secuencia</span><span class="sxs-lookup"><span data-stu-id="a7d02-554">Sequence</span></span> | <span data-ttu-id="a7d02-555">Type</span><span class="sxs-lookup"><span data-stu-id="a7d02-555">Type</span></span>      | <span data-ttu-id="a7d02-556">Datos</span><span class="sxs-lookup"><span data-stu-id="a7d02-556">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="a7d02-557">0</span><span class="sxs-lookup"><span data-stu-id="a7d02-557">0</span></span>        | <span data-ttu-id="a7d02-558">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="a7d02-558">Text node</span></span> | <span data-ttu-id="a7d02-559">Second</span><span class="sxs-lookup"><span data-stu-id="a7d02-559">Second</span></span> |

<span data-ttu-id="a7d02-560">Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:</span><span class="sxs-lookup"><span data-stu-id="a7d02-560">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="a7d02-561">Cambie el valor del primer nodo de texto a `Second`.</span><span class="sxs-lookup"><span data-stu-id="a7d02-561">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="a7d02-562">Quite el segundo nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="a7d02-562">Remove the second text node.</span></span>

<span data-ttu-id="a7d02-563">La generación de los números de secuencia ha perdido toda la información útil `if/else` sobre dónde estaban presentes las bifurcaciones y los bucles en el código original.</span><span class="sxs-lookup"><span data-stu-id="a7d02-563">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="a7d02-564">Esto da como resultado una diferencia **dos veces mayor que** antes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-564">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="a7d02-565">Este es un ejemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="a7d02-565">This is a trivial example.</span></span> <span data-ttu-id="a7d02-566">En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento es más grave.</span><span class="sxs-lookup"><span data-stu-id="a7d02-566">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="a7d02-567">En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación y, normalmente, compilar scripts de edición mucho más largos porque está informada sobre cómo las estructuras antiguas y nuevas se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="a7d02-567">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="a7d02-568">Instrucciones y conclusiones</span><span class="sxs-lookup"><span data-stu-id="a7d02-568">Guidance and conclusions</span></span>

* <span data-ttu-id="a7d02-569">El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-569">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="a7d02-570">El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-570">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="a7d02-571">No escriba grandes bloques de lógica implementada `RenderTreeBuilder` manualmente.</span><span class="sxs-lookup"><span data-stu-id="a7d02-571">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="a7d02-572">Prefiere `.razor` archivos y permite al compilador tratar con los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="a7d02-572">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="a7d02-573">Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor.</span><span class="sxs-lookup"><span data-stu-id="a7d02-573">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="a7d02-574">El valor inicial y los huecos son irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="a7d02-574">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="a7d02-575">Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido).</span><span class="sxs-lookup"><span data-stu-id="a7d02-575">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="a7d02-576">El uso de los números de secuencia es más rápido, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan.</span><span class="sxs-lookup"><span data-stu-id="a7d02-576">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="a7d02-577">La diferenciación es mucho más rápida cuando se utilizan números de secuencia y increíble tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para `.razor` los programadores que crean archivos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-577">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="a7d02-578">Localización</span><span class="sxs-lookup"><span data-stu-id="a7d02-578">Localization</span></span>

<span data-ttu-id="a7d02-579">Las aplicaciones de servidor increíblemente ligeras se localizan con [middleware de localización](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="a7d02-579">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="a7d02-580">El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-580">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="a7d02-581">La referencia cultural se puede establecer utilizando uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a7d02-581">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="a7d02-582">Cookies</span><span class="sxs-lookup"><span data-stu-id="a7d02-582">Cookies</span></span>](#cookies)
* [<span data-ttu-id="a7d02-583">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="a7d02-583">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="a7d02-584">Para obtener más información y ejemplos, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a7d02-584">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="a7d02-585">Cookies</span><span class="sxs-lookup"><span data-stu-id="a7d02-585">Cookies</span></span>

<span data-ttu-id="a7d02-586">Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-586">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="a7d02-587">La cookie se crea mediante el `OnGet` método de la página host de la aplicación (*pages/host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="a7d02-587">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="a7d02-588">El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-588">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="a7d02-589">El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="a7d02-589">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="a7d02-590">Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="a7d02-590">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="a7d02-591">Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="a7d02-591">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="a7d02-592">Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización.</span><span class="sxs-lookup"><span data-stu-id="a7d02-592">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="a7d02-593">Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-593">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="a7d02-594">En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización.</span><span class="sxs-lookup"><span data-stu-id="a7d02-594">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="a7d02-595">Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación del lado servidor increíblemente alta:</span><span class="sxs-lookup"><span data-stu-id="a7d02-595">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="a7d02-596">La localización se controla en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a7d02-596">Localization is handled in the app:</span></span>

1. <span data-ttu-id="a7d02-597">El explorador envía una solicitud HTTP inicial a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7d02-597">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="a7d02-598">El middleware de localización asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="a7d02-598">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="a7d02-599">El `OnGet` método en *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a7d02-599">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="a7d02-600">El explorador abre una conexión WebSocket para crear una sesión de servidor increíblemente interactiva.</span><span class="sxs-lookup"><span data-stu-id="a7d02-600">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="a7d02-601">El middleware de localización lee la cookie y asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="a7d02-601">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="a7d02-602">La sesión de servidor extraordinaria se inicia con la referencia cultural correcta.</span><span class="sxs-lookup"><span data-stu-id="a7d02-602">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="a7d02-603">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="a7d02-603">Provide UI to choose the culture</span></span>

<span data-ttu-id="a7d02-604">Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* .</span><span class="sxs-lookup"><span data-stu-id="a7d02-604">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="a7d02-605">El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso&mdash;seguro al que se redirige al usuario a una página de inicio de sesión y, a continuación, se redirige al recurso original.</span><span class="sxs-lookup"><span data-stu-id="a7d02-605">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="a7d02-606">La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador.</span><span class="sxs-lookup"><span data-stu-id="a7d02-606">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="a7d02-607">El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.</span><span class="sxs-lookup"><span data-stu-id="a7d02-607">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="a7d02-608">Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:</span><span class="sxs-lookup"><span data-stu-id="a7d02-608">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="a7d02-609">Use el `LocalRedirect` resultado de la acción para evitar ataques de redireccionamiento abiertos.</span><span class="sxs-lookup"><span data-stu-id="a7d02-609">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="a7d02-610">Para obtener más información, consulta <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="a7d02-610">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="a7d02-611">El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="a7d02-611">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="a7d02-612">Uso de escenarios de localización de .NET en aplicaciones increíbles</span><span class="sxs-lookup"><span data-stu-id="a7d02-612">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="a7d02-613">Dentro de las aplicaciones increíbles, están disponibles los siguientes escenarios de localización y globalización de .NET:</span><span class="sxs-lookup"><span data-stu-id="a7d02-613">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="a7d02-614">. Sistema de recursos de la red</span><span class="sxs-lookup"><span data-stu-id="a7d02-614">.NET's resources system</span></span>
* <span data-ttu-id="a7d02-615">Formato de fecha y número específico de la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="a7d02-615">Culture-specific number and date formatting</span></span>

<span data-ttu-id="a7d02-616">La funcionalidad de `@bind` extraordinarias realiza la globalización en función de la referencia cultural actual del usuario.</span><span class="sxs-lookup"><span data-stu-id="a7d02-616">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="a7d02-617">Para obtener más información, vea la sección [enlace de datos](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="a7d02-617">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="a7d02-618">Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a7d02-618">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="a7d02-619">`IStringLocalizer<>`*es compatible* con aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="a7d02-619">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="a7d02-620">`IHtmlLocalizer<>`, `IViewLocalizer<>`y la localización de anotaciones de datos son ASP.net Core escenarios MVC y **no se admiten** en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="a7d02-620">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="a7d02-621">Para obtener más información, consulta <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a7d02-621">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="a7d02-622">Imágenes de gráficos vectoriales escalables (SVG)</span><span class="sxs-lookup"><span data-stu-id="a7d02-622">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="a7d02-623">Dado que el increíble procesador representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*) `<img>` , se admiten a través de la etiqueta:</span><span class="sxs-lookup"><span data-stu-id="a7d02-623">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="a7d02-624">Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="a7d02-624">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="a7d02-625">Sin embargo, el marcado SVG en línea no se admite en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="a7d02-625">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="a7d02-626">Si coloca una `<svg>` etiqueta directamente en un archivo de componente ( *. Razor*), se admite la representación de imagen básica, pero aún no se admiten muchos escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="a7d02-626">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="a7d02-627">Por ejemplo, `<use>` las etiquetas no se respetan `@bind` actualmente y no se pueden usar con algunas etiquetas SVG.</span><span class="sxs-lookup"><span data-stu-id="a7d02-627">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="a7d02-628">Esperamos abordar estas limitaciones en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="a7d02-628">We expect to address these limitations in a future release.</span></span>