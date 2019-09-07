---
title: integración de gRPC Client Factory en .NET Core
author: jamesnk
description: Aprenda a crear clientes de gRPC mediante el generador de cliente.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/clientfactory
ms.openlocfilehash: a52fd397a7ed3e327938b0847af7f4e6a4a79400
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310611"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="cf2ca-103">integración de gRPC Client Factory en .NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2ca-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="cf2ca-104">la integración de `HttpClientFactory` gRPC con ofrece una manera centralizada de crear clientes de gRPC.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="cf2ca-105">Se puede usar como alternativa a [la configuración de instancias de cliente de gRPC independientes](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="cf2ca-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="cf2ca-106">La integración de fábrica está disponible en el paquete NuGet [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="cf2ca-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="cf2ca-107">El generador ofrece las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="cf2ca-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="cf2ca-108">Proporciona una ubicación central para configurar las instancias de cliente gRPC lógicas</span><span class="sxs-lookup"><span data-stu-id="cf2ca-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="cf2ca-109">Administra la duración del`HttpClientMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="cf2ca-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="cf2ca-110">Propagación automática de fecha límite y cancelación en un servicio de gRPC de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2ca-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="cf2ca-111">Registro de clientes de gRPC</span><span class="sxs-lookup"><span data-stu-id="cf2ca-111">Register gRPC clients</span></span>

<span data-ttu-id="cf2ca-112">Para registrar un cliente de gRPC, se `AddGrpcClient` puede usar el método de extensión `Startup.ConfigureServices`genérico en, especificando la clase de cliente con tipo de gRPC y la dirección de servicio:</span><span class="sxs-lookup"><span data-stu-id="cf2ca-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("http://localhost:5001");
});
```

<span data-ttu-id="cf2ca-113">El tipo de cliente gRPC se registra como transitorio con la inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="cf2ca-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="cf2ca-114">Ahora el cliente se puede insertar y consumir directamente en los tipos creados por DI.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="cf2ca-115">Los controladores de ASP.NET Core MVC, los concentradores de Signalr y los servicios de gRPC son lugares donde se pueden insertar automáticamente los clientes de gRPC:</span><span class="sxs-lookup"><span data-stu-id="cf2ca-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a><span data-ttu-id="cf2ca-116">Configuración de HttpClient</span><span class="sxs-lookup"><span data-stu-id="cf2ca-116">Configure HttpClient</span></span>

<span data-ttu-id="cf2ca-117">`HttpClientFactory`crea el `HttpClient` que usa el cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="cf2ca-118">Los `HttpClientFactory` métodos estándar se pueden usar para agregar middleware de solicitud de salida o para `HttpClientHandler` configurar el `HttpClient`subyacente de:</span><span class="sxs-lookup"><span data-stu-id="cf2ca-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.BaseAddress = new Uri("http://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

<span data-ttu-id="cf2ca-119">Para obtener más información, consulte [Make HTTP requests Using IHttpClientFactory](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="cf2ca-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="cf2ca-120">Configurar el canal y los interceptores</span><span class="sxs-lookup"><span data-stu-id="cf2ca-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="cf2ca-121">los métodos específicos de gRPC están disponibles para:</span><span class="sxs-lookup"><span data-stu-id="cf2ca-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="cf2ca-122">Configure el canal subyacente de un cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="cf2ca-123">Agregue `Interceptor` instancias que el cliente usará al realizar llamadas a gRPC.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("http://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="cf2ca-124">Fecha límite y propagación de cancelación</span><span class="sxs-lookup"><span data-stu-id="cf2ca-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="cf2ca-125">los clientes de gRPC creados por el generador en un servicio de gRPC se `EnableCallContextPropagation()` pueden configurar con para propagar automáticamente la fecha límite y el token de cancelación a las llamadas secundarias.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="cf2ca-126">El `EnableCallContextPropagation()` método de extensión está disponible en el paquete NuGet [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="cf2ca-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="cf2ca-127">La propagación de contexto de llamada funciona mediante la lectura de la fecha límite y el token de cancelación del contexto de solicitud gRPC actual y su propagación automática a las llamadas salientes realizadas por el cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="cf2ca-128">La propagación de contexto de llamada es una excelente manera de garantizar que los escenarios complejos de gRPC anidados siempre propagan la fecha límite y la cancelación.</span><span class="sxs-lookup"><span data-stu-id="cf2ca-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("http://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="cf2ca-129">Para obtener más información acerca de las fechas límite y la cancelación de RPC, consulte [ciclo de vida de RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span><span class="sxs-lookup"><span data-stu-id="cf2ca-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf2ca-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cf2ca-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>