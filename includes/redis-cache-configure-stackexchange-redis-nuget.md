---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 8ebf5ddfa118e0aeadeab0c00a981871a4b5708e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132907"
---
Las aplicaciones .NET pueden usar el cliente de caché **StackExchange.Redis** , que se puede configurar en Visual Studio mediante un paquete NuGet que simplifica la configuración de aplicaciones de cliente de caché. 

> [!NOTE]
> Para obtener más información, vea la página de GitHub [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) y la [documentación del cliente de StackExchange.Azure Cache for Redis](http://github.com/StackExchange/StackExchange.Redis#documentation).
>
>

Para configurar una aplicación cliente en Visual Studio usando el paquete NuGet StackExchange.Redis, haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Administrar paquetes NuGet**. 

![Manage NuGet packages](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Escriba **StackExchange.Redis** o **StackExchange.Redis.StrongName** en el cuadro de texto de búsqueda, seleccione la versión que desee en los resultados y haga clic en **Instalar**.

> [!NOTE]
> Si prefiere usar una versión con nombre seguro de la biblioteca de cliente **StackExchange.Redis**, elija **StackExchange.Redis.StrongName**; de lo contrario, elija **StackExchange.Redis**.
>
>

![StackExchange.Redis NuGet package](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

El paquete NuGet descarga y agrega las referencias de ensamblado requeridas para que la aplicación cliente acceda a Azure Cache for Redis con el cliente StackExchange.Azure Cache for Redis.

> [!NOTE]
> Si anteriormente ha configurado el proyecto para usar StackExchange.Redis, puede buscar actualizaciones para el paquete desde el **Administrador de paquetes NuGet**. Para buscar e instalar versiones actualizadas del paquete StackExchange.Redis NuGet, haga clic en **Actualizaciones** en la ventana del **Administrador de paquetes NuGet**. Si existe una actualización para el paquete StackExchange.Redis de NuGet, puede actualizar el proyecto para usar la versión actualizada.
>
>

También puede instalar el paquete NuGet StackExchange.Redis haciendo clic en **Administrador de paquetes NuGet**, **Consola del Administrador de paquetes** desde el menú **Herramientas** y ejecutando el comando siguiente en la ventana **Consola del Administrador de paquetes**.

```
Install-Package StackExchange.Redis
```
