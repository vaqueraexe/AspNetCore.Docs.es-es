---
title: Configuración del enlazador de ASP.NET Core Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/configure-linker
ms.openlocfilehash: efac62c325b03b5ee19cae58abb5227f3d300b69
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103431"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="23c9b-103">Configuración del enlazador de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="23c9b-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="23c9b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="23c9b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

Blazor<span data-ttu-id="23c9b-105"> WebAssembly realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilación para reducir el IL innecesario de los ensamblados de salida de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c9b-105"> WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="23c9b-106">El enlazador está deshabilitado durante la compilación en la configuración Debug.</span><span class="sxs-lookup"><span data-stu-id="23c9b-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="23c9b-107">Las aplicaciones deben compilarse en la configuración Release para habilitar el enlazador.</span><span class="sxs-lookup"><span data-stu-id="23c9b-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="23c9b-108">Se recomienda compilar en Release cuando se implementen las aplicaciones Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="23c9b-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="23c9b-109">La vinculación de una aplicación optimiza el tamaño, pero puede tener efectos perjudiciales.</span><span class="sxs-lookup"><span data-stu-id="23c9b-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="23c9b-110">Las aplicaciones que usan la reflexión o características dinámicas relacionadas pueden verse interrumpidas cuando se reduzcan porque el enlazador no conoce este comportamiento dinámico y no puede determinar, en general, qué tipos son necesarios para la reflexión en el entorno de ejecución.</span><span class="sxs-lookup"><span data-stu-id="23c9b-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="23c9b-111">Para reducir estas aplicaciones, se debe informar al vinculador de los tipos necesarios para la reflexión en el código y en los paquetes o marcos de trabajo de los que depende la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c9b-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="23c9b-112">Para asegurarse de que la aplicación reducida funciona correctamente una vez implementada, es importante probar las compilaciones de Release de la aplicación con frecuencia durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="23c9b-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="23c9b-113">La vinculación de las aplicaciones de Blazor se puede configurar con estas características de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="23c9b-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="23c9b-114">Configuración de la vinculación global con una [propiedad de MSBuild](#control-linking-with-an-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="23c9b-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="23c9b-115">Control de la vinculación por cada ensamblado con un [archivo de configuración](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="23c9b-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="23c9b-116">Control de la vinculación con una propiedad de MSBuild</span><span class="sxs-lookup"><span data-stu-id="23c9b-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="23c9b-117">La vinculación se habilita cuando una aplicación se compila en la configuración `Release`.</span><span class="sxs-lookup"><span data-stu-id="23c9b-117">Linking is enabled when an app is built in `Release` configuration.</span></span> <span data-ttu-id="23c9b-118">Para cambiar esto, configure la propiedad `BlazorWebAssemblyEnableLinking` de MSBuild en el archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="23c9b-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="23c9b-119">Control de la vinculación con un archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="23c9b-119">Control linking with a configuration file</span></span>

<span data-ttu-id="23c9b-120">Control de la vinculación por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="23c9b-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="LinkerConfig.xml" />
</ItemGroup>
```

<span data-ttu-id="23c9b-121">*LinkerConfig.xml*:</span><span class="sxs-lookup"><span data-stu-id="23c9b-121">*LinkerConfig.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="23c9b-122">Para obtener más información y ejemplos, vea [Formatos de datos (repositorio mono/linker de GitHub)](https://github.com/mono/linker/blob/master/docs/data-formats.md).</span><span class="sxs-lookup"><span data-stu-id="23c9b-122">For more information and examples, see [Data Formats (mono/linker GitHub repository)](https://github.com/mono/linker/blob/master/docs/data-formats.md).</span></span>

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a><span data-ttu-id="23c9b-123">Adición de un archivo de configuración del enlazador XML a una biblioteca</span><span class="sxs-lookup"><span data-stu-id="23c9b-123">Add an XML linker configuration file to a library</span></span>

<span data-ttu-id="23c9b-124">Para configurar el enlazador para una biblioteca específica, agregue un archivo de configuración de enlazador XML a la biblioteca como un recurso incrustado.</span><span class="sxs-lookup"><span data-stu-id="23c9b-124">To configure the linker for a specific library, add an XML linker configuration file into the library as an embedded resource.</span></span> <span data-ttu-id="23c9b-125">El recurso incrustado debe tener el mismo nombre que el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="23c9b-125">The embedded resource must have the same name as the assembly.</span></span>

<span data-ttu-id="23c9b-126">En el ejemplo siguiente, el archivo *LinkerConfig.xml* se especifica como un recurso incrustado que tiene el mismo nombre que el ensamblado de la biblioteca:</span><span class="sxs-lookup"><span data-stu-id="23c9b-126">In the following example, the *LinkerConfig.xml* file is specified as an embedded resource that has the same name as the library's assembly:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="23c9b-127">Configuración del enlazador para la internacionalización</span><span class="sxs-lookup"><span data-stu-id="23c9b-127">Configure the linker for internationalization</span></span>

<span data-ttu-id="23c9b-128">De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones WebAssembly de Blazor quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="23c9b-128">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="23c9b-129">Al quitar estos ensamblados se minimiza el tamaño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c9b-129">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="23c9b-130">Para controlar qué ensamblados de I18N se conservan, establezca la propiedad `<BlazorWebAssemblyI18NAssemblies>` de MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="23c9b-130">To control which I18N assemblies are retained, set the `<BlazorWebAssemblyI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyI18NAssemblies>{all|none|REGION1,REGION2,...}</BlazorWebAssemblyI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="23c9b-131">Valor de región</span><span class="sxs-lookup"><span data-stu-id="23c9b-131">Region Value</span></span>     | <span data-ttu-id="23c9b-132">Ensamblado de región de Mono</span><span class="sxs-lookup"><span data-stu-id="23c9b-132">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="23c9b-133">Todos los ensamblados incluidos</span><span class="sxs-lookup"><span data-stu-id="23c9b-133">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="23c9b-134">*I18N.CJK.dll*</span><span class="sxs-lookup"><span data-stu-id="23c9b-134">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="23c9b-135">*I18N.MidEast.dll*</span><span class="sxs-lookup"><span data-stu-id="23c9b-135">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="23c9b-136">`none` (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="23c9b-136">`none` (default)</span></span> | <span data-ttu-id="23c9b-137">None</span><span class="sxs-lookup"><span data-stu-id="23c9b-137">None</span></span>                    |
| `other`          | <span data-ttu-id="23c9b-138">*I18N.Other.dll*</span><span class="sxs-lookup"><span data-stu-id="23c9b-138">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="23c9b-139">*I18N.Rare.dll*</span><span class="sxs-lookup"><span data-stu-id="23c9b-139">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="23c9b-140">*I18N.West.dll*</span><span class="sxs-lookup"><span data-stu-id="23c9b-140">*I18N.West.dll*</span></span>         |

<span data-ttu-id="23c9b-141">Use una coma para separar varios valores (por ejemplo, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="23c9b-141">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="23c9b-142">Para más información, vea [I18N: biblioteca del marco de internacionalización Pnetlib (repositorio de GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="23c9b-142">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23c9b-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="23c9b-143">Additional resources</span></span>

* <xref:blazor/webassembly-performance-best-practices#intermediate-language-il-linking>