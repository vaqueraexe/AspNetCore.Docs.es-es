---
title: Novedades de ASP.NET Core 3.0
author: rick-anderson
description: Obtenga información sobre las nuevas características de ASP.NET Core 3.0.
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: aspnetcore-3.0
ms.openlocfilehash: 490d00da7282e2efe28fcc52e593dd71d7324d3f
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198993"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="d2fc0-103">Novedades de ASP.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="d2fc0-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="d2fc0-104">En este artículo se resaltan los cambios más importantes de ASP.NET Core 3.0, con vínculos a la documentación pertinente.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## <a name="blazor"></a><span data-ttu-id="d2fc0-105">Blazor</span><span class="sxs-lookup"><span data-stu-id="d2fc0-105">Blazor</span></span>

<span data-ttu-id="d2fc0-106">Blazor es un marco nuevo en ASP.NET Core para la creación de interfaces de usuario web interactivas del lado cliente con .NET:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-106">Blazor is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="d2fc0-107">Cree interfaces de usuario completamente interactivas con C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="d2fc0-108">Comparta la lógica de aplicación del lado cliente y servidor escrita con .NET.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="d2fc0-109">Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="d2fc0-110">Escenarios compatibles con el marco Blazor:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-110">Blazor framework supported scenarios:</span></span>

* <span data-ttu-id="d2fc0-111">Componentes de interfaz de usuario reutilizables (componentes de Razor)</span><span class="sxs-lookup"><span data-stu-id="d2fc0-111">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="d2fc0-112">Enrutamiento del lado cliente</span><span class="sxs-lookup"><span data-stu-id="d2fc0-112">Client-side routing</span></span>
* <span data-ttu-id="d2fc0-113">Diseños de componentes</span><span class="sxs-lookup"><span data-stu-id="d2fc0-113">Component layouts</span></span>
* <span data-ttu-id="d2fc0-114">Compatibilidad con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="d2fc0-114">Support for dependency injection</span></span>
* <span data-ttu-id="d2fc0-115">Formularios y validación</span><span class="sxs-lookup"><span data-stu-id="d2fc0-115">Forms and validation</span></span>
* <span data-ttu-id="d2fc0-116">Compilar bibliotecas de componentes con bibliotecas de clases de Razor</span><span class="sxs-lookup"><span data-stu-id="d2fc0-116">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="d2fc0-117">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2fc0-117">JavaScript interop</span></span>

<span data-ttu-id="d2fc0-118">Para más información, consulte <xref:blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-118">For more information, see <xref:blazor/index>.</span></span>

### <a name="blazor-server"></a><span data-ttu-id="d2fc0-119">Servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="d2fc0-119">Blazor Server</span></span>

<span data-ttu-id="d2fc0-120">Blazor separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-120">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="d2fc0-121">El servidor Blazor ofrece compatibilidad con el hospedaje de componentes de Razor en el servidor en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-121">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="d2fc0-122">Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-122">UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="d2fc0-123">El servidor Blazor es compatible con ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-123">Blazor Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="blazor-webassembly-preview"></a><span data-ttu-id="d2fc0-124">WebAssembly de Blazor (versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="d2fc0-124">Blazor WebAssembly (Preview)</span></span>

<span data-ttu-id="d2fc0-125">Las aplicaciones Blazor también se pueden ejecutar directamente en el explorador mediante un entorno de ejecución .NET basado en WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-125">Blazor apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> <span data-ttu-id="d2fc0-126">WebAssembly de Blazor se encuentra en versión preliminar *no* se admite en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-126">Blazor WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> <span data-ttu-id="d2fc0-127">WebAssembly de Blazor se admitirá en una versión futura de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-127">Blazor WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="d2fc0-128">Componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="d2fc0-128">Razor components</span></span>

<span data-ttu-id="d2fc0-129">Las aplicaciones Blazor se crean a partir de componentes.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-129">Blazor apps are built from components.</span></span> <span data-ttu-id="d2fc0-130">Los componentes son fragmentos independientes de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-130">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="d2fc0-131">Los componentes son clases .NET normales que definen la lógica de representación de la interfaz de usuario y los controladores de eventos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-131">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="d2fc0-132">Puede crear aplicaciones web interactivas enriquecidas sin JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-132">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="d2fc0-133">Los componentes de Blazor normalmente se crean mediante la sintaxis de Razor, una mezcla natural de HTML y C#.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-133">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="d2fc0-134">Los componentes de Razor son similares a las vistas de Razor Pages y MVC en que ambos usan Razor.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-134">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="d2fc0-135">A diferencia de las páginas y las vistas, que se basan en un modelo de solicitud y respuesta, los componentes se usan específicamente para controlar la composición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-135">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="d2fc0-136">gRPC</span><span class="sxs-lookup"><span data-stu-id="d2fc0-136">gRPC</span></span>

<span data-ttu-id="d2fc0-137">[gRPC](https://grpc.io/):</span><span class="sxs-lookup"><span data-stu-id="d2fc0-137">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="d2fc0-138">Es un conocido marco RPC (llamada a procedimiento remoto) de alto rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-138">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="d2fc0-139">Ofrece un enfoque dogmático de contrato primero para el desarrollo de API.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-139">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="d2fc0-140">Utiliza tecnologías modernas como estas:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-140">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="d2fc0-141">HTTP/2 para el transporte.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-141">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="d2fc0-142">Búferes de protocolo como el lenguaje de descripción de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-142">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="d2fc0-143">Formato de serialización binario.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-143">Binary serialization format.</span></span>
* <span data-ttu-id="d2fc0-144">Proporciona características como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-144">Provides features such as:</span></span>

  * <span data-ttu-id="d2fc0-145">Autenticación</span><span class="sxs-lookup"><span data-stu-id="d2fc0-145">Authentication</span></span>
  * <span data-ttu-id="d2fc0-146">Streaming y control de flujo bidireccionales.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-146">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="d2fc0-147">Cancelación y tiempos de espera.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-147">Cancellation and timeouts.</span></span>

<span data-ttu-id="d2fc0-148">La funcionalidad gRPC en ASP.NET Core 3.0 incluye:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-148">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="d2fc0-149">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; Un marco ASP.NET Core para hospedar servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-149">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="d2fc0-150">gRPC en ASP.NET Core se integra con características de ASP.NET Core estándar como el registro, la inserción de dependencias (DI), la autenticación y la autorización.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-150">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication and authorization.</span></span>
* <span data-ttu-id="d2fc0-151">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; Un cliente de gRPC para .NET Core que se basa en la conocida clase `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-151">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="d2fc0-152">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; Integración del cliente de gRPC con `HttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-152">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="d2fc0-153">Para más información, consulte <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-153">For more information, see <xref:grpc/index>.</span></span>

## <a name="signalr"></a><span data-ttu-id="d2fc0-154">SignalR</span><span class="sxs-lookup"><span data-stu-id="d2fc0-154">SignalR</span></span>

<span data-ttu-id="d2fc0-155">Consulte [Actualizar el código de SignalR](xref:migration/22-to-30#signalr) para obtener instrucciones sobre la migración.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-155">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> <span data-ttu-id="d2fc0-156">SignalR ahora usa `System.Text.Json` para serializar o deserializar los mensajes JSON.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-156">SignalR now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="d2fc0-157">Consulte [Cambiar a Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) para obtener instrucciones sobre cómo restaurar el serializador basado en `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-157">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="d2fc0-158">En los clientes de JavaScript y .NET para SignalR, se agregó compatibilidad para la reconexión automática.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-158">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="d2fc0-159">De forma predeterminada, el cliente intenta conectarse de nuevo inmediatamente y lo vuelve a intentar después de dos, diez y treinta segundos si es necesario.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-159">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="d2fc0-160">Si el cliente se vuelve a conectar correctamente, recibe un nuevo identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-160">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="d2fc0-161">La reconexión automática es opcional:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-161">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="d2fc0-162">Los intervalos de reconexión se pueden especificar pasando una matriz de duraciones basadas en milisegundos:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-162">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="d2fc0-163">Se puede pasar una implementación personalizada para el control total de los intervalos de reconexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-163">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="d2fc0-164">Si se produce un error en la reconexión después del último intervalo de reconexión:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-164">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="d2fc0-165">El cliente considera que la conexión está desconectada.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-165">The client considers the connection is offline.</span></span>
* <span data-ttu-id="d2fc0-166">El cliente deja de intentar la reconexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-166">The client stops trying to reconnect.</span></span>

<span data-ttu-id="d2fc0-167">Durante los intentos de reconexión, actualice la interfaz de usuario de la aplicación para notificar al usuario que se está intentando la reconexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-167">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="d2fc0-168">Para proporcionar comentarios sobre la interfaz de usuario cuando la conexión se interrumpe, la API de cliente de SignalR se ha ampliado para incluir los siguientes controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-168">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="d2fc0-169">`onreconnecting`:  ofrece a los desarrolladores una oportunidad para deshabilitar la interfaz de usuario o para que los usuarios sepan que la aplicación está sin conexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-169">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="d2fc0-170">`onreconnected`: ofrece a los desarrolladores una oportunidad para actualizar la interfaz de usuario una vez que se restablece la conexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-170">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="d2fc0-171">En el código siguiente se usa `onreconnecting` para actualizar la interfaz de usuario mientras se intenta conectar:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-171">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="d2fc0-172">En el código siguiente se usa `onreconnected` para actualizar la interfaz de usuario en la conexión:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-172">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="d2fc0-173">SignalR 3.0 y las versiones posteriores proporcionan un recurso personalizado a los controladores de autorización cuando un método Hub requiere autorización.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-173">SignalR 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="d2fc0-174">El recurso es una instancia de `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-174">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="d2fc0-175">`HubInvocationContext` incluye:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-175">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="d2fc0-176">Nombre del método Hub que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-176">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="d2fc0-177">Argumentos para el método Hub.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-177">Arguments to the hub method.</span></span>

<span data-ttu-id="d2fc0-178">Considere el siguiente ejemplo de una aplicación de salón de chat que permite el inicio de sesión de varias organizaciones a través de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-178">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="d2fc0-179">Cualquier persona con un cuenta de Microsoft puede iniciar sesión en el chat, pero solo los miembros de la organización propietaria pueden prohibir usuarios o ver los historiales de chat de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users’ chat histories.</span></span> <span data-ttu-id="d2fc0-180">La aplicación podría restringir ciertas funcionalidades de usuarios específicos.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-180">The app could restrict certain functionality from specific users.</span></span>

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="d2fc0-181">En el código anterior, `DomainRestrictedRequirement` actúa como `IAuthorizationRequirement` personalizado.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-181">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="d2fc0-182">Dado que se pasa el parámetro de recurso `HubInvocationContext`, la lógica interna puede:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-182">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="d2fc0-183">Inspeccionar el contexto en el que se llama al método Hub.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-183">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="d2fc0-184">Tomar decisiones sobre cómo permitir que el usuario ejecute métodos Hub individuales.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-184">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="d2fc0-185">Los métodos Hub individuales se pueden decorar con el nombre de la directiva que el código comprueba en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-185">Individual Hub methods can be decorated with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="d2fc0-186">Cuando los clientes intentan llamar a métodos Hub individuales, el controlador `DomainRestrictedRequirement` se ejecuta y controla el acceso a los métodos.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-186">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="d2fc0-187">En función de la forma en que `DomainRestrictedRequirement` controla el acceso:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-187">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="d2fc0-188">Todos los usuarios que han iniciado sesión pueden llamar al método `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-188">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="d2fc0-189">Solo los usuarios que han iniciado sesión con una dirección de correo electrónico `@jabbr.net` pueden ver los historiales de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-189">Only users who have logged in with a `@jabbr.net` email address can view users’ histories.</span></span>
* <span data-ttu-id="d2fc0-190">Solo `bob42@jabbr.net` puede prohibir a los usuarios del salón de chat.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-190">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

<span data-ttu-id="d2fc0-191">La creación de la directiva `DomainRestricted` puede implicar:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-191">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="d2fc0-192">En *Startup.cs*, agregar la nueva directiva.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-192">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="d2fc0-193">Proporcionar el requisito de `DomainRestrictedRequirement` personalizado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-193">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="d2fc0-194">Registrar `DomainRestricted` con el middleware de autorización.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-194">Registering `DomainRestricted` with the authorization middleware.</span></span>

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

<span data-ttu-id="d2fc0-195">Los métodos Hub de SignalR usan el [enrutamiento de punto de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-195">SignalR hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="d2fc0-196">La conexión de los métodos Hub de SignalR se hizo previamente explícitamente:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-196">SignalR hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="d2fc0-197">En la versión anterior, los desarrolladores debían conectar los controladores, las páginas de Razor y los métodos Hub en distintos lugares.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-197">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of different places.</span></span> <span data-ttu-id="d2fc0-198">La conexión explícita da lugar a una serie de segmentos de enrutamiento casi idénticos:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-198">Explicit connection results in a series of nearly-identical routing segments:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

<span data-ttu-id="d2fc0-199">Los métodos Hub de SignalR 3.0 se pueden enrutar a través del enrutamiento de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-199">SignalR 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="d2fc0-200">Con el enrutamiento de punto de conexión, normalmente todo el enrutamiento se puede configurar en `UseRouting`:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-200">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="d2fc0-201">SignalR 3.0 de ASP.NET Core agregado:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-201">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="d2fc0-202">Streaming de cliente a servidor.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-202">Client-to-server streaming.</span></span> <span data-ttu-id="d2fc0-203">Con el streaming de cliente a servidor, los métodos del lado servidor pueden tomar instancias de `IAsyncEnumerable<T>` o `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-203">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="d2fc0-204">En el ejemplo de C# siguiente, el método `UploadStream` del método Hub recibirá un flujo de cadenas del cliente:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-204">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="d2fc0-205">Las aplicaciones del cliente .NET pueden pasar una instancia de `IAsyncEnumerable<T>` o `ChannelReader<T>` como el argumento `stream` del método Hub `UploadStream` anterior.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-205">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="d2fc0-206">Una vez que el bucle `for` se ha completado y la función local se cierra, se envía la finalización de la secuencia:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-206">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="d2fc0-207">Las aplicaciones cliente de JavaScript usan `Subject` de SignalR (o [Subject de RxJS ](https://rxjs.dev/api/index/class/Subject)) para el argumento `stream` del método Hub `UploadStream` anterior.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-207">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="d2fc0-208">El código de JavaScript podría usar el método `subject.next` para controlar las cadenas a medida que se capturan y están listas para enviarse al servidor.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-208">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="d2fc0-209">Mediante el uso de código como los dos fragmentos de código anteriores, se pueden crear experiencias de streaming en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-209">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

### <a name="new-json-serialization"></a><span data-ttu-id="d2fc0-210">Nueva serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="d2fc0-210">New JSON serialization</span></span>

<span data-ttu-id="d2fc0-211">ASP.NET Core 3.0 ahora usa <xref:System.Text.Json> de forma predeterminada para la serialización de JSON:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-211">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="d2fc0-212">Lee y escribe JSON de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-212">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="d2fc0-213">Está optimizado para texto UTF-8.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-213">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="d2fc0-214">Normalmente, mayor rendimiento que `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-214">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="d2fc0-215">Para agregar Json.NET a ASP.NET Core 3.0, consulte [Adición de compatibilidad con el formato JSON basado en Newtonsoft.Json](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-215">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="d2fc0-216">Nuevas directivas de Razor</span><span class="sxs-lookup"><span data-stu-id="d2fc0-216">New Razor directives</span></span>

<span data-ttu-id="d2fc0-217">La lista siguiente contiene las nuevas directivas de Razor:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-217">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="d2fc0-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; La directiva `@attribute` aplica el atributo especificado a la clase de la página o vista generada.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="d2fc0-219">Por ejemplo: `@attribute [Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-219">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="d2fc0-220">[@implements](xref:mvc/views/razor#implements) &ndash; La directiva `@implements` implementa una interfaz para la clase generada.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-220">[@implements](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="d2fc0-221">Por ejemplo: `@implements IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-221">For example, `@implements IDisposable`.</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="d2fc0-222">Autenticación de certificados y Kerberos</span><span class="sxs-lookup"><span data-stu-id="d2fc0-222">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="d2fc0-223">La autenticación de certificados requiere:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-223">Certificate authentication requires:</span></span>

* <span data-ttu-id="d2fc0-224">Configurar el servidor para que acepte certificados.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-224">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="d2fc0-225">Agregar el middleware de autenticación en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-225">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="d2fc0-226">Agregar el servicio de autenticación de certificados en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-226">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="d2fc0-227">Entre las opciones de autenticación de certificados se incluye la capacidad de:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-227">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="d2fc0-228">Aceptar certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-228">Accept self-signed certificates.</span></span>
* <span data-ttu-id="d2fc0-229">Comprobar la revocación de certificados.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-229">Check for certificate revocation.</span></span>
* <span data-ttu-id="d2fc0-230">Comprobar que el certificado ofrecido tiene las marcas de uso correctas.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-230">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="d2fc0-231">Una entidad de seguridad de usuario predeterminada se construye a partir de las propiedades del certificado.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-231">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="d2fc0-232">La entidad de seguridad de usuario contiene un evento que permite complementar o reemplazar la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-232">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="d2fc0-233">Para más información, consulte <xref:security/authentication/certauth>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-233">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="d2fc0-234">La [autenticación de Windows](/windows-server/security/windows-authentication/windows-authentication-overview) se ha ampliado a Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-234">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="d2fc0-235">En versiones anteriores, la autenticación de Windows se limitaba a [IIS](xref:host-and-deploy/iis/index) y [HttpSys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-235">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="d2fc0-236">En ASP.NET Core 3,0, [Kestrel](xref:fundamentals/servers/kestrel) tiene la capacidad de usar Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview) y [NTLM en Windows](/windows-server/security/kerberos/ntlm-overview), Linux y macOS para hosts unidos a un dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-236">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="d2fc0-237">La compatibilidad con Kestrel de estos esquemas de autenticación se proporciona mediante el paquete [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-237">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="d2fc0-238">Al igual que con los demás servicios de autenticación, configure la autenticación en toda la aplicación y luego configure el servicio:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-238">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="d2fc0-239">Requisitos del host:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-239">Host requirements:</span></span>

* <span data-ttu-id="d2fc0-240">Los hosts de Windows deben tener [nombres de entidad de seguridad de servicio](/windows/win32/ad/service-principal-names) (SPN) agregados a la cuenta de usuario que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-240">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="d2fc0-241">Las máquinas Linux y macOS deben estar unidas al dominio.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-241">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="d2fc0-242">Los SPN se deben crear para el proceso web.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-242">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="d2fc0-243">Los [archivos keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) se deben generar y configurar en la máquina host.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-243">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="d2fc0-244">Para más información, consulte <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-244">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="d2fc0-245">Cambios en la plantilla</span><span class="sxs-lookup"><span data-stu-id="d2fc0-245">Template changes</span></span>

<span data-ttu-id="d2fc0-246">Se ha eliminado lo siguiente de las plantillas de la interfaz de usuario web (Razor Pages, MVC con el controlador y las vistas):</span><span class="sxs-lookup"><span data-stu-id="d2fc0-246">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="d2fc0-247">La interfaz de usuario de consentimiento de cookies ya no está incluida.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-247">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="d2fc0-248">Para habilitar la función de consentimiento de cookies en una aplicación generada por plantillas de ASP.NET Core 3.0, consulte <xref:security/gdpr>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-248">To enable the cookie consent feature in an ASP.NET Core 3.0 template generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="d2fc0-249">Ahora se hace referencia a los scripts y los recursos estáticos relacionados como archivos locales en lugar de usar CDN.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-249">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="d2fc0-250">Para más información, consulte [Ahora se hace referencia a los scripts y recursos estáticos relacionados como archivos locales en lugar de usar CDN en base al entorno actual (aspnet/AspNetCore.Docs nº 14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-250">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="d2fc0-251">La plantilla de Angular se ha actualizado para usar Angular 8.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-251">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="d2fc0-252">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-252">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="d2fc0-253">Una nueva opción de plantilla en Visual Studio proporciona compatibilidad con plantillas para páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-253">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="d2fc0-254">Al crear una biblioteca de clases de Razor a partir de la plantilla en un shell de comandos, pase la opción `-support-pages-and-views` (`dotnet new razorclasslib -support-pages-and-views`).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-254">When creating an RCL from the template in a command shell, pass the `-support-pages-and-views` option (`dotnet new razorclasslib -support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="d2fc0-255">Host genérico</span><span class="sxs-lookup"><span data-stu-id="d2fc0-255">Generic Host</span></span>

<span data-ttu-id="d2fc0-256">Las plantillas de ASP.NET Core 3.0 usan <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-256">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="d2fc0-257">Las versiones anteriores usaban <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-257">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="d2fc0-258">El uso del host genérico de .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) proporciona una mejor integración de las aplicaciones ASP.NET Core con otros escenarios de servidor que no son específicos de la Web.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-258">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that are not web specific.</span></span> <span data-ttu-id="d2fc0-259">Para más información, vea [HostBuilder reemplaza a WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-259">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="d2fc0-260">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="d2fc0-260">Host configuration</span></span>

<span data-ttu-id="d2fc0-261">Antes de la versión de ASP.NET Core 3.0, se cargaron las variables de entorno prefijadas con `ASPNETCORE_` para la configuración del host web.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-261">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="d2fc0-262">En la versión 3.0, `AddEnvironmentVariables` se usaba para cargar variables de entorno con el prefijo `DOTNET_` para la configuración de host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-262">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-contructor-injection"></a><span data-ttu-id="d2fc0-263">Cambios en la inserción del constructor Startup</span><span class="sxs-lookup"><span data-stu-id="d2fc0-263">Changes to Startup contructor injection</span></span>

<span data-ttu-id="d2fc0-264">El host genérico solo admite los siguientes tipos para la inserción del constructor `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-264">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="d2fc0-265">Todos los servicios se pueden seguir insertando directamente como argumentos en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-265">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="d2fc0-266">Para más información, consulte [El host genérico restringe la inserción del constructor Startup (aspnet/Announcements nº 353)](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-266">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="d2fc0-267">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d2fc0-267">Kestrel</span></span>

* <span data-ttu-id="d2fc0-268">La configuración de Kestrel se ha actualizado para la migración al host genérico.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-268">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="d2fc0-269">En la versión 3.0, Kestrel se configura en el generador de hosts web proporcionado por `ConfigureWebHostDefaults`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-269">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="d2fc0-270">Los adaptadores de conexión se han quitado de Kestrel y se han reemplazado por el middleware de conexión, que es similar al middleware HTTP en la canalización de ASP.NET Core, pero para conexiones de nivel inferior.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-270">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="d2fc0-271">La capa de transporte de Kestrel se ha expuesto como una interfaz pública en `Connections.Abstractions`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-271">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="d2fc0-272">La ambigüedad entre encabezados y finalizadores se ha resuelto moviendo los encabezados finales a una nueva colección.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-272">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="d2fc0-273">Las API de E/S síncronas, como `HttpReqeuest.Body.Read`, son una fuente común de ausencia de subprocesos que provocan bloqueos en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-273">Synchronous IO APIs, such as `HttpReqeuest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="d2fc0-274">En la versión 3.0, `AllowSynchronousIO` se ha deshabilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-274">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="d2fc0-275">Para más información, consulte <xref:migration/22-to-30#kestrel>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-275">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="d2fc0-276">HTTP/2 habilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-276">HTTP/2 enabled by default</span></span>

<span data-ttu-id="d2fc0-277">HTTP/2 está habilitado de forma predeterminada en Kestrel para puntos finales HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-277">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="d2fc0-278">La compatibilidad con HTTP/2 para IIS o HTTP.sys está habilitada cuando el sistema operativo la admite.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-278">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="request-counters"></a><span data-ttu-id="d2fc0-279">Contadores de solicitudes</span><span class="sxs-lookup"><span data-stu-id="d2fc0-279">Request counters</span></span>

<span data-ttu-id="d2fc0-280">El tipo EventSource de hospedaje (Microsoft.AspNetCore.Hosting) emite los siguientes tipos de EventCounter relacionados con las solicitudes entrantes:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-280">The Hosting EventSource (Microsoft.AspNetCore.Hosting) emits the following EventCounters related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="d2fc0-281">Enrutamiento de puntos de conexión</span><span class="sxs-lookup"><span data-stu-id="d2fc0-281">Endpoint routing</span></span>

<span data-ttu-id="d2fc0-282">El enrutamiento de puntos de conexión, que permite que los marcos (por ejemplo, MVC) funcionen bien con middleware, se ha mejorado:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-282">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="d2fc0-283">El orden de los puntos de conexión y el middleware es configurable en la canalización de procesamiento de solicitudes de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-283">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="d2fc0-284">Los extremos y el middleware se componen bien con otras tecnologías basadas en ASP.NET Core, como las comprobaciones de estado.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-284">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="d2fc0-285">Los puntos finales pueden implementar una directiva, como CORS o la autorización, tanto en el middleware como en MVC.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-285">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="d2fc0-286">Los filtros y atributos se pueden colocar en métodos en los controladores.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-286">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="d2fc0-287">Para más información, consulte <xref:fundamentals/routing#routing-basics>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-287">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="d2fc0-288">Comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="d2fc0-288">Health Checks</span></span>

<span data-ttu-id="d2fc0-289">Las comprobaciones de estado utilizan el enrutamiento de puntos de conexión con el host genérico.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-289">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="d2fc0-290">En `Startup.Configure`, llame a `MapHealthChecks` en el generador de puntos de conexiones con la dirección URL del punto de conexión o la ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-290">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="d2fc0-291">Los puntos de conexión de las comprobaciones de estado pueden:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-291">Health Checks endpoints can:</span></span>

* <span data-ttu-id="d2fc0-292">Especificar uno o más hosts o puertos permitidos.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-292">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="d2fc0-293">Requerir autorización.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-293">Require authorization.</span></span>
* <span data-ttu-id="d2fc0-294">Requerir CORS.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-294">Require CORS.</span></span>

<span data-ttu-id="d2fc0-295">Para obtener más información, vea los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-295">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="d2fc0-296">Canalizaciones en HttpContext</span><span class="sxs-lookup"><span data-stu-id="d2fc0-296">Pipes on HttpContext</span></span>

<span data-ttu-id="d2fc0-297">Ahora es posible leer el cuerpo de la solicitud y escribir el cuerpo de la respuesta mediante la API <xref:System.IO.Pipelines>.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-297">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="d2fc0-298">A la clase</span><span class="sxs-lookup"><span data-stu-id="d2fc0-298">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="d2fc0-299">La propiedad `HttpRequest.BodyReader` proporciona <xref:System.IO.Pipelines.PipeReader> que se puede usar para leer el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-299">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="d2fc0-300">A la clase</span><span class="sxs-lookup"><span data-stu-id="d2fc0-300">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="d2fc0-301">La propiedad `HttpResponse.BodyWriter` proporciona <xref:System.IO.Pipelines.PipeWriter> que se puede usar para escribir el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-301">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="d2fc0-302">`HttpRequest.BodyReader` es análogo al flujo `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-302">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="d2fc0-303">`HttpResponse.BodyWriter` es análogo al flujo `HttpResponse.Body`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-303">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="d2fc0-304">Informes de errores mejorados en IIS</span><span class="sxs-lookup"><span data-stu-id="d2fc0-304">Improved error reporting in IIS</span></span>

<span data-ttu-id="d2fc0-305">Los errores de inicio al hospedar aplicaciones ASP.NET Core en IIS ahora producen datos de diagnóstico más completos.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-305">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="d2fc0-306">Estos errores se informan al registro de eventos de Windows con seguimientos de pila siempre que proceda.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-306">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="d2fc0-307">Además, todas las advertencias, los errores y las excepciones no controladas se registran en el registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-307">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="d2fc0-308">Servicio de trabajo y SDK de trabajo</span><span class="sxs-lookup"><span data-stu-id="d2fc0-308">Worker Service and Worker SDK</span></span>

<span data-ttu-id="d2fc0-309">.NET Core 3.0 presenta la nueva plantilla de la aplicación de servicio de trabajo.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-309">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="d2fc0-310">Esta plantilla proporciona un punto de partida para escribir aplicaciones de servicio de larga duración en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-310">This template is provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="d2fc0-311">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-311">For more information, see:</span></span>

* [<span data-ttu-id="d2fc0-312">Trabajos de .NET Core como servicios de Windows</span><span class="sxs-lookup"><span data-stu-id="d2fc0-312">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="d2fc0-313">Mejoras del middleware de encabezados reenviados</span><span class="sxs-lookup"><span data-stu-id="d2fc0-313">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="d2fc0-314">En versiones anteriores de ASP.NET Core, llamar a <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> y <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> era problemático cuando se implementaba en Azure Linux o detrás de cualquier proxy inverso que no fuera IIS.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-314">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="d2fc0-315">La corrección para las versiones anteriores se documenta en [Reenvío del esquema para servidores proxy inversos Linux y que no son de IIS](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-315">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="d2fc0-316">Este escenario se ha corregido en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-316">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="d2fc0-317">El host posibilita [middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) cuando la variable de entorno `ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-317">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="d2fc0-318">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true` en nuestras imágenes de contenedor.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-318">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="d2fc0-319">Mejoras en el rendimiento</span><span class="sxs-lookup"><span data-stu-id="d2fc0-319">Performance improvements</span></span>

<span data-ttu-id="d2fc0-320">ASP.NET Core 3.0 incluye muchas mejoras que reducen el uso de memoria y aumentan el rendimiento:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-320">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="d2fc0-321">Reducción del uso de memoria cuando se usa el contenedor de inserción de dependencias integrado para los servicios de ámbito.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-321">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="d2fc0-322">Reducción de las asignaciones en el marco, incluidos los escenarios de middleware y el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-322">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="d2fc0-323">Reducción del uso de memoria para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-323">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="d2fc0-324">Reducción de memoria y mejoras de rendimiento para las conexiones HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-324">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="d2fc0-325">Nuevo serializador JSON optimizado y totalmente asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-325">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="d2fc0-326">Reducción del uso de memoria y mejoras de rendimiento en el análisis de formularios.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-326">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="d2fc0-327">ASP.NET Core 3.0 solo se ejecuta en .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="d2fc0-327">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="d2fc0-328">A partir de ASP.NET Core 3.0, .NET Framework ya no es un marco de destino admitido.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-328">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="d2fc0-329">Los proyectos que tienen .NET Framework como destino pueden continuar usando la [versión .NET Core 2.1 LTS](https://www.microsoft.com/net/download/dotnet-core/2.1) de forma totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-329">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="d2fc0-330">La mayoría de los paquetes relacionados con ASP.NET Core 2.1. x se admitirán de forma indefinida, más allá del período LTS de 3 años para .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-330">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the 3 year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="d2fc0-331">Para información sobre la migración, consulte [Realice la portabilidad de su código de .NET Framework a .NET Core](/dotnet/core/porting/).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-331">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="d2fc0-332">Uso del marco compartido de .NET Core</span><span class="sxs-lookup"><span data-stu-id="d2fc0-332">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="d2fc0-333">El marco compartido de ASP.NET Core 3.0, incluido en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), ya no requiere un elemento `<PackageReference />` explícito en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-333">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="d2fc0-334">Se hace referencia al marco compartido automáticamente al usar el SDK `Microsoft.NET.Sdk.Web` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-334">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="d2fc0-335">Ensamblados quitados del marco compartido de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2fc0-335">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="d2fc0-336">Los ensamblados más importantes que se han quitado del marco compartido de ASP.NET Core 3.0 son:</span><span class="sxs-lookup"><span data-stu-id="d2fc0-336">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="d2fc0-337">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-337">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="d2fc0-338">Para agregar Json.NET a ASP.NET Core 3.0, consulte [Adición de compatibilidad con el formato JSON basado en Newtonsoft.Json](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-338">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="d2fc0-339">ASP.NET Core 3.0 presenta `System.Text.Json` para leer y escribir JSON.</span><span class="sxs-lookup"><span data-stu-id="d2fc0-339">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="d2fc0-340">Para más información, consulte [Nueva serialización de JSON](#new-json-serialization).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-340">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="d2fc0-341">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d2fc0-341">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="d2fc0-342">Para una lista completa de los ensamblados que se han quitado del marco compartido, consulte [Ensamblados quitados de Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-342">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="d2fc0-343">Para más información sobre la motivación de este cambio, consulte [Últimos cambios en Microsoft.AspNetCore.App en 3.0](https://github.com/aspnet/Announcements/issues/325) y [Un primer vistazo a los cambios que vienen en ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="d2fc0-343">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->