---
title: Aplicación de una directiva de seguridad de contenido para Blazor de ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar una directiva de seguridad de contenido con aplicaciones Blazor de ASP.NET Core para protegerse de ataques de scripts de sitios (XSS).
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
uid: blazor/security/content-security-policy
ms.openlocfilehash: 63359260766846ccf14ececfae82bf02cc598cf9
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103424"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-blazor"></a><span data-ttu-id="266b3-103">Aplicación de una directiva de seguridad de contenido para Blazor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="266b3-103">Enforce a Content Security Policy for ASP.NET Core Blazor</span></span>

<span data-ttu-id="266b3-104">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="266b3-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="266b3-105">Los [ataques de scripts de sitios (XSS)](xref:security/cross-site-scripting) son una vulnerabilidad de seguridad en la que un atacante coloca uno o más scripts de cliente malintencionados en el contenido representado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="266b3-105">[Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) is a security vulnerability where an attacker places one or more malicious client-side scripts into an app's rendered content.</span></span> <span data-ttu-id="266b3-106">Una directiva de seguridad de contenido ayuda a protegerse de ataques XSS, ya que informa al explorador de la validez de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="266b3-106">A Content Security Policy (CSP) helps protect against XSS attacks by informing the browser of valid:</span></span>

* <span data-ttu-id="266b3-107">Orígenes de contenido cargado, incluidos scripts, hojas de estilo e imágenes</span><span class="sxs-lookup"><span data-stu-id="266b3-107">Sources for loaded content, including scripts, stylesheets, and images.</span></span>
* <span data-ttu-id="266b3-108">Acciones realizadas por una página, especificando los destinos de URL permitidos de formularios</span><span class="sxs-lookup"><span data-stu-id="266b3-108">Actions taken by a page, specifying permitted URL targets of forms.</span></span>
* <span data-ttu-id="266b3-109">Complementos que se pueden cargar</span><span class="sxs-lookup"><span data-stu-id="266b3-109">Plugins that can be loaded.</span></span>

<span data-ttu-id="266b3-110">Para aplicar una directiva de seguridad de contenido en una aplicación, el desarrollador especifica varias *directivas* de seguridad de contenido en uno o varios encabezados `Content-Security-Policy` o etiquetas `<meta>`.</span><span class="sxs-lookup"><span data-stu-id="266b3-110">To apply a CSP to an app, the developer specifies several CSP content security *directives* in one or more `Content-Security-Policy` headers or `<meta>` tags.</span></span>

<span data-ttu-id="266b3-111">El explorador evalúa las directivas mientras una página se carga.</span><span class="sxs-lookup"><span data-stu-id="266b3-111">Policies are evaluated by the browser while a page is loading.</span></span> <span data-ttu-id="266b3-112">Así, inspecciona los orígenes de la página y determina si cumplen los requisitos de las directivas de seguridad de contenido.</span><span class="sxs-lookup"><span data-stu-id="266b3-112">The browser inspects the page's sources and determines if they meet the requirements of the content security directives.</span></span> <span data-ttu-id="266b3-113">Cuando un recurso no cumple estas directivas, el explorador no lo carga.</span><span class="sxs-lookup"><span data-stu-id="266b3-113">When policy directives aren't met for a resource, the browser doesn't load the resource.</span></span> <span data-ttu-id="266b3-114">Pensemos, por ejemplo, en una directiva que no permite scripts de terceros.</span><span class="sxs-lookup"><span data-stu-id="266b3-114">For example, consider a policy that doesn't allow third-party scripts.</span></span> <span data-ttu-id="266b3-115">Si una página contiene una etiqueta `<script>` con un origen de terceros en el atributo `src`, el explorador impide que el script se cargue.</span><span class="sxs-lookup"><span data-stu-id="266b3-115">When a page contains a `<script>` tag with a third-party origin in the `src` attribute, the browser prevents the script from loading.</span></span>

<span data-ttu-id="266b3-116">Las directivas de seguridad de contenido se admiten en la mayoría de los exploradores de escritorio y móviles de hoy día, como Chrome, Edge, Firefox, Opera y Safari.</span><span class="sxs-lookup"><span data-stu-id="266b3-116">CSP is supported in most modern desktop and mobile browsers, including Chrome, Edge, Firefox, Opera, and Safari.</span></span> <span data-ttu-id="266b3-117">Se recomienda usarlas en aplicaciones Blazor.</span><span class="sxs-lookup"><span data-stu-id="266b3-117">CSP is recommended for Blazor apps.</span></span>

## <a name="policy-directives"></a><span data-ttu-id="266b3-118">Directivas</span><span class="sxs-lookup"><span data-stu-id="266b3-118">Policy directives</span></span>

<span data-ttu-id="266b3-119">Especifique como mínimo las siguientes directivas y orígenes en las aplicaciones Blazor.</span><span class="sxs-lookup"><span data-stu-id="266b3-119">Minimally, specify the following directives and sources for Blazor apps.</span></span> <span data-ttu-id="266b3-120">Agregue más directivas y orígenes según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="266b3-120">Add additional directives and sources as needed.</span></span> <span data-ttu-id="266b3-121">Las siguientes directivas se usan en la sección [Aplicación de la directiva](#apply-the-policy) de este artículo, donde se proporcionan directivas de seguridad de ejemplo para WebAssembly de Blazor y Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="266b3-121">The following directives are used in the [Apply the policy](#apply-the-policy) section of this article, where example security policies for Blazor WebAssembly and Blazor Server are provided:</span></span>

* <span data-ttu-id="266b3-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri): restringe las direcciones URL según la etiqueta `<base>` de una página.</span><span class="sxs-lookup"><span data-stu-id="266b3-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri): Restricts the URLs for a page's `<base>` tag.</span></span> <span data-ttu-id="266b3-123">Especifique `self` para indicar que el origen de la aplicación (incluido el esquema y el número de puerto) es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="266b3-123">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="266b3-124">[block-all-mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content): impide la carga de contenido HTTP y HTTPS combinado.</span><span class="sxs-lookup"><span data-stu-id="266b3-124">[block-all-mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content): Prevents loading mixed HTTP and HTTPS content.</span></span>
* <span data-ttu-id="266b3-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src): indica una reserva de las directivas de origen que no se han especificado expresamente en la directiva.</span><span class="sxs-lookup"><span data-stu-id="266b3-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src): Indicates a fallback for source directives that aren't explicitly specified by the policy.</span></span> <span data-ttu-id="266b3-126">Especifique `self` para indicar que el origen de la aplicación (incluido el esquema y el número de puerto) es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="266b3-126">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="266b3-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src): indica los orígenes de imágenes válidos.</span><span class="sxs-lookup"><span data-stu-id="266b3-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src): Indicates valid sources for images.</span></span>
  * <span data-ttu-id="266b3-128">Especifique `data:` para permitir la carga de imágenes de direcciones URL `data:`.</span><span class="sxs-lookup"><span data-stu-id="266b3-128">Specify `data:` to permit loading images from `data:` URLs.</span></span>
  * <span data-ttu-id="266b3-129">Especifique `https:` para permitir la carga de imágenes desde puntos de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="266b3-129">Specify `https:` to permit loading images from HTTPS endpoints.</span></span>
* <span data-ttu-id="266b3-130">[object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src): indica los orígenes válidos de las etiquetas `<object>`, `<embed>` y `<applet>`.</span><span class="sxs-lookup"><span data-stu-id="266b3-130">[object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src): Indicates valid sources for the `<object>`, `<embed>`, and `<applet>` tags.</span></span> <span data-ttu-id="266b3-131">Especifique `none` para no permitir ningún origen de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="266b3-131">Specify `none` to prevent all URL sources.</span></span>
* <span data-ttu-id="266b3-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src): indica los orígenes de scripts válidos.</span><span class="sxs-lookup"><span data-stu-id="266b3-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src): Indicates valid sources for scripts.</span></span>
  * <span data-ttu-id="266b3-133">Especifique el origen de host `https://stackpath.bootstrapcdn.com/` para los scripts de arranque.</span><span class="sxs-lookup"><span data-stu-id="266b3-133">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap scripts.</span></span>
  * <span data-ttu-id="266b3-134">Especifique `self` para indicar que el origen de la aplicación (incluido el esquema y el número de puerto) es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="266b3-134">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="266b3-135">En una aplicación WebAssembly de Blazor:</span><span class="sxs-lookup"><span data-stu-id="266b3-135">In a Blazor WebAssembly app:</span></span>
    * <span data-ttu-id="266b3-136">Especifique los siguientes hash para permitir la carga de scripts en línea de WebAssembly de Blazor necesarios:</span><span class="sxs-lookup"><span data-stu-id="266b3-136">Specify the following hashes to permit the required Blazor WebAssembly inline scripts to load:</span></span>
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * <span data-ttu-id="266b3-137">Especifique `unsafe-eval` para usar `eval()` y métodos para crear código a partir de cadenas.</span><span class="sxs-lookup"><span data-stu-id="266b3-137">Specify `unsafe-eval` to use `eval()` and methods for creating code from strings.</span></span>
  * <span data-ttu-id="266b3-138">En una aplicación Blazor Server, especifique el hash `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` para el script en línea que realiza la detección de reserva en hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="266b3-138">In a Blazor Server app, specify the `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` hash for the inline script that performs fallback detection for stylesheets.</span></span>
* <span data-ttu-id="266b3-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src): indica los orígenes de hojas de estilo válidos.</span><span class="sxs-lookup"><span data-stu-id="266b3-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src): Indicates valid sources for stylesheets.</span></span>
  * <span data-ttu-id="266b3-140">Especifique el origen de host `https://stackpath.bootstrapcdn.com/` para las hojas de estilo de arranque.</span><span class="sxs-lookup"><span data-stu-id="266b3-140">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap stylesheets.</span></span>
  * <span data-ttu-id="266b3-141">Especifique `self` para indicar que el origen de la aplicación (incluido el esquema y el número de puerto) es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="266b3-141">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="266b3-142">Especifique `unsafe-inline` para permitir el uso de estilos en línea.</span><span class="sxs-lookup"><span data-stu-id="266b3-142">Specify `unsafe-inline` to allow the use of inline styles.</span></span> <span data-ttu-id="266b3-143">La declaración en línea es necesaria en la interfaz de usuario de las aplicaciones Blazor Server para volver a conectar el cliente y el servidor después de la solicitud inicial.</span><span class="sxs-lookup"><span data-stu-id="266b3-143">The inline declaration is required for the UI in Blazor Server apps for reconnecting the client and server after the initial request.</span></span> <span data-ttu-id="266b3-144">En una versión futura, es posible que los estilos en línea se eliminen, de forma que `unsafe-inline` ya no sea necesario.</span><span class="sxs-lookup"><span data-stu-id="266b3-144">In a future release, inline styling might be removed so that `unsafe-inline` is no longer required.</span></span>
* <span data-ttu-id="266b3-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests): indica que las direcciones URL de contenido de orígenes (HTTP) no seguros deben adquirirse de forma segura a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="266b3-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests): Indicates that content URLs from insecure (HTTP) sources should be acquired securely over HTTPS.</span></span>

<span data-ttu-id="266b3-146">Todos los exploradores, excepto Microsoft Internet Explorer, admiten las directivas anteriores.</span><span class="sxs-lookup"><span data-stu-id="266b3-146">The preceding directives are supported by all browsers except Microsoft Internet Explorer.</span></span>

<span data-ttu-id="266b3-147">Para obtener los hash SHA de otros scripts en línea:</span><span class="sxs-lookup"><span data-stu-id="266b3-147">To obtain SHA hashes for additional inline scripts:</span></span>

* <span data-ttu-id="266b3-148">Aplique la directiva de seguridad de contenido que aparece en la sección [Aplicación de la directiva](#apply-the-policy).</span><span class="sxs-lookup"><span data-stu-id="266b3-148">Apply the CSP shown in the [Apply the policy](#apply-the-policy) section.</span></span>
* <span data-ttu-id="266b3-149">Acceda a la consola de herramientas de desarrollo del explorador mientras ejecuta la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="266b3-149">Access the browser's developer tools console while running the app locally.</span></span> <span data-ttu-id="266b3-150">El explorador calcula y muestra los hash de los scripts bloqueados cuando existe un encabezado de directiva de seguridad de contenido o una etiqueta `meta`.</span><span class="sxs-lookup"><span data-stu-id="266b3-150">The browser calculates and displays hashes for blocked scripts when a CSP header or `meta` tag is present.</span></span>
* <span data-ttu-id="266b3-151">Copie los hash proporcionados por el explorador en los orígenes `script-src`.</span><span class="sxs-lookup"><span data-stu-id="266b3-151">Copy the hashes provided by the browser to the `script-src` sources.</span></span> <span data-ttu-id="266b3-152">Inserte cada hash entre comillas simples.</span><span class="sxs-lookup"><span data-stu-id="266b3-152">Use single quotes around each hash.</span></span>

<span data-ttu-id="266b3-153">Para obtener una matriz de compatibilidad de exploradores de nivel 2 de directiva de seguridad de contenido, vea [¿Puedo usar una directiva de seguridad de contenido de nivel 2?](https://www.caniuse.com/#feat=contentsecuritypolicy2)</span><span class="sxs-lookup"><span data-stu-id="266b3-153">For a Content Security Policy Level 2 browser support matrix, see [Can I use: Content Security Policy Level 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span></span>

## <a name="apply-the-policy"></a><span data-ttu-id="266b3-154">Aplicación de la directiva</span><span class="sxs-lookup"><span data-stu-id="266b3-154">Apply the policy</span></span>

<span data-ttu-id="266b3-155">Use una etiqueta `<meta>` para aplicar la directiva:</span><span class="sxs-lookup"><span data-stu-id="266b3-155">Use a `<meta>` tag to apply the policy:</span></span>

* <span data-ttu-id="266b3-156">Establezca el valor del atributo `http-equiv` en `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="266b3-156">Set the value of the `http-equiv` attribute to `Content-Security-Policy`.</span></span>
* <span data-ttu-id="266b3-157">Coloque las directivas en el valor de atributo `content`.</span><span class="sxs-lookup"><span data-stu-id="266b3-157">Place the directives in the `content` attribute value.</span></span> <span data-ttu-id="266b3-158">Separe las directivas con un punto y coma (`;`).</span><span class="sxs-lookup"><span data-stu-id="266b3-158">Separate directives with a semicolon (`;`).</span></span>
* <span data-ttu-id="266b3-159">Coloque siempre la etiqueta `meta` en el contenido de `<head>`.</span><span class="sxs-lookup"><span data-stu-id="266b3-159">Always place the `meta` tag in the `<head>` content.</span></span>

<span data-ttu-id="266b3-160">En las siguientes secciones se muestran directivas de ejemplo de WebAssembly de Blazor y Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="266b3-160">The following sections show example policies for Blazor WebAssembly and Blazor Server.</span></span> <span data-ttu-id="266b3-161">Estos ejemplos pertenecen a la versión de Blazor correspondiente a este artículo.</span><span class="sxs-lookup"><span data-stu-id="266b3-161">These examples are versioned with this article for each release of Blazor.</span></span> <span data-ttu-id="266b3-162">Para usar la versión adecuada en su caso, seleccione la versión del documento usando el selector desplegable **Versión** de esta página web.</span><span class="sxs-lookup"><span data-stu-id="266b3-162">To use a version appropriate for your release, select the document version with the **Version** drop down selector on this webpage.</span></span>

### <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="266b3-163"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="266b3-163"> WebAssembly</span></span>

<span data-ttu-id="266b3-164">En el contenido de `<head>` de la página host *wwwroot/index.html*, aplique las directivas descritas en la sección [Directivas](#policy-directives):</span><span class="sxs-lookup"><span data-stu-id="266b3-164">In the `<head>` content of the *wwwroot/index.html* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="blazor-server"></a><span data-ttu-id="266b3-165">Servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="266b3-165">Blazor Server</span></span>

<span data-ttu-id="266b3-166">En el contenido de `<head>` de la página host *Pages/_Host.cshtml*, aplique las directivas descritas en la sección [Directivas](#policy-directives):</span><span class="sxs-lookup"><span data-stu-id="266b3-166">In the `<head>` content of the *Pages/_Host.cshtml* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a><span data-ttu-id="266b3-167">Limitaciones de la etiqueta meta</span><span class="sxs-lookup"><span data-stu-id="266b3-167">Meta tag limitations</span></span>

<span data-ttu-id="266b3-168">Una directiva de etiqueta `<meta>` no admite las siguientes directivas:</span><span class="sxs-lookup"><span data-stu-id="266b3-168">A `<meta>` tag policy doesn't support the following directives:</span></span>

* [<span data-ttu-id="266b3-169">frame-ancestors</span><span class="sxs-lookup"><span data-stu-id="266b3-169">frame-ancestors</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [<span data-ttu-id="266b3-170">report-to</span><span class="sxs-lookup"><span data-stu-id="266b3-170">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="266b3-171">report-uri</span><span class="sxs-lookup"><span data-stu-id="266b3-171">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [<span data-ttu-id="266b3-172">sandbox</span><span class="sxs-lookup"><span data-stu-id="266b3-172">sandbox</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

<span data-ttu-id="266b3-173">Para admitir las directivas anteriores, use un encabezado denominado `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="266b3-173">To support the preceding directives, use a header named `Content-Security-Policy`.</span></span> <span data-ttu-id="266b3-174">La cadena de directiva es el valor del encabezado.</span><span class="sxs-lookup"><span data-stu-id="266b3-174">The directive string is the header's value.</span></span>

## <a name="test-a-policy-and-receive-violation-reports"></a><span data-ttu-id="266b3-175">Comprobación de una directiva y recepción de informes de infracciones</span><span class="sxs-lookup"><span data-stu-id="266b3-175">Test a policy and receive violation reports</span></span>

<span data-ttu-id="266b3-176">Las comprobaciones ayudan a confirmar que no se han bloqueado scripts de terceros por error al crear una directiva inicial.</span><span class="sxs-lookup"><span data-stu-id="266b3-176">Testing helps confirm that third-party scripts aren't inadvertently blocked when building an initial policy.</span></span>

<span data-ttu-id="266b3-177">Para comprobar una directiva durante un período de tiempo sin aplicar directivas, establezca el atributo `http-equiv` de la etiqueta `<meta>` o el nombre de encabezado de una directiva basada en encabezado en `Content-Security-Policy-Report-Only`.</span><span class="sxs-lookup"><span data-stu-id="266b3-177">To test a policy over a period of time without enforcing the policy directives, set the `<meta>` tag's `http-equiv` attribute or header name of a header-based policy to `Content-Security-Policy-Report-Only`.</span></span> <span data-ttu-id="266b3-178">Los informes de errores se envían como documentos JSON a la dirección URL que se especifique.</span><span class="sxs-lookup"><span data-stu-id="266b3-178">Failure reports are sent as JSON documents to a specified URL.</span></span> <span data-ttu-id="266b3-179">Para obtener más información, vea [Documentación web de MDN: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span><span class="sxs-lookup"><span data-stu-id="266b3-179">For more information, see [MDN web docs: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span></span>

<span data-ttu-id="266b3-180">Para obtener informes de infracciones mientras una directiva está activa, vea los siguientes artículos:</span><span class="sxs-lookup"><span data-stu-id="266b3-180">For reporting on violations while a policy is active, see the following articles:</span></span>

* [<span data-ttu-id="266b3-181">report-to</span><span class="sxs-lookup"><span data-stu-id="266b3-181">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="266b3-182">report-uri</span><span class="sxs-lookup"><span data-stu-id="266b3-182">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

<span data-ttu-id="266b3-183">Aunque ya no se recomienda el uso de `report-uri`, ambas directivas deben usarse hasta que todos los exploradores principales admitan `report-to`.</span><span class="sxs-lookup"><span data-stu-id="266b3-183">Although `report-uri` is no longer recommended for use, both directives should be used until `report-to` is supported by all of the major browsers.</span></span> <span data-ttu-id="266b3-184">No use `report-uri` única y exclusivamente, ya que la compatibilidad con `report-uri` tiene visos de eliminarse de los exploradores *en cualquier momento*.</span><span class="sxs-lookup"><span data-stu-id="266b3-184">Don't exclusively use `report-uri` because support for `report-uri` is subject to being dropped *at any time* from browsers.</span></span> <span data-ttu-id="266b3-185">Quite la compatibilidad con `report-uri` de sus directivas cuando `report-to` sea totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="266b3-185">Remove support for `report-uri` in your policies when `report-to` is fully supported.</span></span> <span data-ttu-id="266b3-186">Para realizar un seguimiento del uso de `report-to`, vea [¿Puedo usar report-to?](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to)</span><span class="sxs-lookup"><span data-stu-id="266b3-186">To track adoption of `report-to`, see [Can I use: report-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span></span>

<span data-ttu-id="266b3-187">Compruebe y actualice la directiva de una aplicación en cada versión.</span><span class="sxs-lookup"><span data-stu-id="266b3-187">Test and update an app's policy every release.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="266b3-188">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="266b3-188">Troubleshoot</span></span>

* <span data-ttu-id="266b3-189">Aparecen errores en la consola de herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="266b3-189">Errors appear in the browser's developer tools console.</span></span> <span data-ttu-id="266b3-190">Los exploradores proporcionan información sobre lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="266b3-190">Browsers provide information about:</span></span>
  * <span data-ttu-id="266b3-191">Elementos que no cumplen la directiva</span><span class="sxs-lookup"><span data-stu-id="266b3-191">Elements that don't comply with the policy.</span></span>
  * <span data-ttu-id="266b3-192">Cómo modificar la directiva para permitir un elemento bloqueado</span><span class="sxs-lookup"><span data-stu-id="266b3-192">How to modify the policy to allow for a blocked item.</span></span>
* <span data-ttu-id="266b3-193">Una directiva es totalmente efectiva únicamente cuando el explorador del cliente admite todas las directivas incluidas.</span><span class="sxs-lookup"><span data-stu-id="266b3-193">A policy is only completely effective when the client's browser supports all of the included directives.</span></span> <span data-ttu-id="266b3-194">Para obtener una matriz de compatibilidad de exploradores actuales, vea [¿Puedo usar Content-Security-Policy?](https://caniuse.com/#search=Content-Security-Policy)</span><span class="sxs-lookup"><span data-stu-id="266b3-194">For a current browser support matrix, see [Can I use: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="266b3-195">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="266b3-195">Additional resources</span></span>

* [<span data-ttu-id="266b3-196">Documentación web de MDN: Content-Security-Policy</span><span class="sxs-lookup"><span data-stu-id="266b3-196">MDN web docs: Content-Security-Policy</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [<span data-ttu-id="266b3-197">Directiva de seguridad de contenido de nivel 2</span><span class="sxs-lookup"><span data-stu-id="266b3-197">Content Security Policy Level 2</span></span>](https://www.w3.org/TR/CSP2/)
* [<span data-ttu-id="266b3-198">Evaluador de directivas de seguridad de contenido de Google</span><span class="sxs-lookup"><span data-stu-id="266b3-198">Google CSP Evaluator</span></span>](https://csp-evaluator.withgoogle.com/)