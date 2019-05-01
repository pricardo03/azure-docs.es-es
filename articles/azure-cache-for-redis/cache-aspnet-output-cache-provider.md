---
title: Proveedor de caché de resultados de ASP.NET para Azure Cache for Redis
description: Obtenga información acerca de cómo almacenar en caché los resultados de página de ASP.NET con Azure Cache for Redis.
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 04/22/2018
ms.author: yegu
ms.openlocfilehash: a93d21b07dc486f743694ee99f60018ed4ef517c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943871"
---
# <a name="aspnet-output-cache-provider-for-azure-cache-for-redis"></a>Proveedor de caché de resultados de ASP.NET para Azure Cache for Redis

El proveedor de la caché de salida de Redis es un mecanismo de almacenamiento fuera de proceso para los datos de la caché de salida. Estos datos resultan necesarios específicamente para respuestas HTTP completas (caché de resultados de la página). El proveedor se conecta al nuevo punto de extensibilidad del proveedor de caché de salida que se introdujo en ASP.NET 4.

Para usar el proveedor de la caché de salida de Redis, configure primero la caché y luego configure la aplicación de ASP.NET mediante el paquete NuGet del proveedor de la caché de salida de Redis. En este tema se proporcionan instrucciones sobre cómo configurar la aplicación para usar el proveedor de la caché de salida de Redis. Para más información acerca de cómo crear y configurar una instancia de Azure Cache for Redis, consulte [Creación de una caché](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Almacenamiento de los resultados de página de ASP.NET en la caché

Para configurar una aplicación cliente en Visual Studio usando el paquete NuGet de estado de sesión de Azure Cache for Redis, en el menú **Herramientas**, haga clic en **Administrador de paquetes NuGet** y en **Consola del Administrador de paquetes**.

Ejecute el siguiente comando desde la ventana `Package Manager Console`.

```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

El paquete NuGet del proveedor de la caché de salida de Redis tiene una dependencia del paquete StackExchange.Redis.StrongName. Si el paquete StackExchange.Redis.StrongName no existe en el proyecto, se instalará. Para obtener más información sobre el paquete de NuGet del proveedor de caché de salida de Redis, vea la página [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) de NuGet.

>[!NOTE]
>Tenga en cuenta que, además del paquete StackExchange.Redis.StrongName con nombre seguro, también está la versión con nombre no seguro de StackExchange.Redis. Si el proyecto usa la versión de StackExchange.Redis no-con nombre seguro deberá desinstalarla; en caso contrario, experimentará los conflictos de nomenclatura en el proyecto. Para obtener más información sobre estos paquetes, consulte [Configuración de los clientes de la caché de .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

El paquete NuGet descarga y agrega las referencias de ensamblado necesarias y agrega la siguiente sección al archivo web.config. Esta sección contiene la configuración necesaria para que la aplicación ASP.NET use el proveedor de memoria caché de salida de Redis.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

Configure los atributos con los valores de la hoja de la caché en el Portal de Microsoft Azure y configure los demás valores según prefiera. Para obtener instrucciones acerca de cómo acceder a las propiedades de la caché, consulte [Configuración de Azure Cache for Redis](cache-configure.md#configure-azure-cache-for-redis-settings).

| Atributo | Type | Valor predeterminado | DESCRIPCIÓN |
| --------- | ---- | ------- | ----------- |
| *host* | string | "localhost" | El nombre de host o la dirección del IP de servidor de Redis |
| *port* | número entero positivo | 6379 (no SSL)<br/>6380 (SSL) | Puerto del servidor de Redis |
| *accessKey* | string | "" | Redis contraseña del servidor cuando está habilitada la autorización de Redis. El valor es una cadena vacía de forma predeterminada, lo que significa que el proveedor de estado de sesión no utiliza cualquier contraseña al conectarse al servidor de Redis. **Si el servidor de Redis está en una red de acceso público como Azure Redis Cache, asegúrese de habilitar la autorización de Redis mejorar la seguridad y proporcionar una contraseña segura.** |
| *ssl* | boolean | **false** | Si desea conectarse al servidor de Redis a través de SSL. Este valor es **false** de forma predeterminada dado que Redis no admite SSL desde el principio. **Si usa Azure Redis Cache que admite SSL desde el principio, no olvide de establézcalo en true para mejorar la seguridad.**<br/><br/>El puerto no SSL está deshabilitado de forma predeterminada para las cachés nuevas. Especificar **true** para esta opción para usar el puerto SSL. Para más información sobre cómo habilitar el puerto no SSL, consulte la sección [Puertos de acceso](cache-configure.md#access-ports) del tema de [Configuración de caché](cache-configure.md). |
| *databaseIdNumber* | número entero positivo | 0 | *Este atributo se puede especificar únicamente a través de web.config o AppSettings.*<br/><br/>Especifique qué base de datos de Redis para usar. |
| *connectionTimeoutInMilliseconds* | número entero positivo | Proporcionado por StackExchange.Redis | Usa para establecer *ConnectTimeout* al crear StackExchange.Redis.ConnectionMultiplexer. |
| *operationTimeoutInMilliseconds* | número entero positivo | Proporcionado por StackExchange.Redis | Usa para establecer *SyncTimeout* al crear StackExchange.Redis.ConnectionMultiplexer. |
| *connectionString* (cadena de conexión válida StackExchange.Redis) | string | *n/a* | Ya sea una referencia de parámetro a AppSettings o web.config, o bien una cadena de conexión válida de StackExchange.Redis. Este atributo puede proporcionar valores para *host*, *puerto*, *accessKey*, *ssl*y otros atributos de StackExchange.Redis. Para obtener una vista más cercana *connectionString*, consulte [configuración connectionString](#setting-connectionstring) en el [atributo notas](#attribute-notes) sección. |
| *settingsClassName*<br/>*settingsMethodName* | string<br/>string | *n/a* | *Estos atributos se pueden especificar a través de web.config o AppSettings.*<br/><br/>Usar estos atributos para proporcionar una cadena de conexión. *settingsClassName* debe ser un nombre de clase completo del ensamblado que contiene el método especificado por *settingsMethodName*.<br/><br/>El método especificado por *settingsMethodName* debe ser público, estático y void (no tome ningún parámetro), con un tipo de valor devuelto de **cadena**. Este método devuelve la cadena de conexión real. |
| *loggingClassName*<br/>*loggingMethodName* | string<br/>string | *n/a* | *Estos atributos se pueden especificar a través de web.config o AppSettings.*<br/><br/>Usar estos atributos para depurar su aplicación proporcionando los registros desde la caché de estado de sesión y salida, junto con los registros de StackExchange.Redis. *loggingClassName* debe ser un nombre de clase completo del ensamblado que contiene el método especificado por *loggingMethodName*.<br/><br/>El método especificado por *loggingMethodName* debe ser público, estático y void (no tome ningún parámetro), con un tipo de valor devuelto de **System.IO.TextWriter**. |
| *applicationName* | string | El nombre del módulo del proceso actual o "/" | *Sólo SessionStateProvider*<br/>*Este atributo se puede especificar únicamente a través de web.config o AppSettings.*<br/><br/>El prefijo del nombre de aplicación para usar en la memoria caché en Redis. El cliente podría usar la misma caché de Redis para distintos fines. Para garantizar que las claves de sesión no entren en conflicto, se puede tener como prefijo el nombre de la aplicación. |
| *throwOnError* | boolean | true | *Sólo SessionStateProvider*<br/>*Este atributo se puede especificar únicamente a través de web.config o AppSettings.*<br/><br/>Si se produce una excepción cuando se produce un error.<br/><br/>Para obtener más información acerca de *throwOnError*, consulte [notas sobre la *throwOnError* ](#notes-on-throwonerror) en el [atributo notas](#attribute-notes) sección. |>*Microsoft.Web.Redis.RedisSessionStateProvider.LastException*. |
| *retryTimeoutInMilliseconds* | número entero positivo | 5000 | *Sólo SessionStateProvider*<br/>*Este atributo se puede especificar únicamente a través de web.config o AppSettings.*<br/><br/>¿Cuánto tiempo para volver a intentar cuando falla una operación. Si este valor es menor que *operationTimeoutInMilliseconds*, el proveedor no volverá a intentar.<br/><br/>Para obtener más información acerca de *retryTimeoutInMilliseconds*, consulte [notas sobre la *retryTimeoutInMilliseconds* ](#notes-on-retrytimeoutinmilliseconds) en el [atributo notas](#attribute-notes) sección. |
| *redisSerializerType* | string | *n/a* | Especifica el nombre de tipo calificado de ensamblado de una clase que implementa Microsoft.Web.Redis. ISerializer y que contiene la lógica personalizada para serializar y deserializar los valores. Para obtener más información, consulte [sobre *redisSerializerType* ](#about-redisserializertype) en el [atributo notas](#attribute-notes) sección. |
|

## <a name="attribute-notes"></a>Notas de atributo

### <a name="setting-connectionstring"></a>Establecer *connectionString*

El valor de *connectionString* se utiliza como clave para capturar la cadena de conexión real de AppSettings, si este tipo de cadena existe en AppSettings. Si no se encuentra dentro de AppSettings, el valor de *connectionString* se utilizará como clave para recuperar la cadena de conexión real desde el archivo web.config **ConnectionString** sección si no existe esa sección. Si la cadena de conexión no existe en el archivo web.config o en AppSettings **ConnectionString** sección, el valor literal de *connectionString* se usará como la cadena de conexión al crear StackExchange.Redis.ConnectionMultiplexer.

Los ejemplos siguientes muestran cómo *connectionString* se utiliza.

#### <a name="example-1"></a>Ejemplo 1

```xml
<connectionStrings>
    <add name="MyRedisConnectionString" connectionString="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</connectionStrings>
```

En `web.config`, utilizar esta clave como valor del parámetro en lugar del valor real.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-2"></a>Ejemplo 2

```xml
<appSettings>
    <add key="MyRedisConnectionString" value="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</appSettings>
```

En `web.config`, utilizar esta clave como valor del parámetro en lugar del valor real.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-3"></a>Ejemplo 3

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False"/>
    </providers>
</sessionState>
```

### <a name="notes-on-throwonerror"></a>Notas sobre *throwOnError*

Actualmente, si se produce un error durante una operación de sesión, el proveedor de estado de sesión producirá una excepción. Esto se cierra la aplicación.

Este comportamiento se modificó de manera que es compatible con las expectativas de los usuarios existentes del proveedor de estado de sesión ASP.NET al tiempo que también proporciona la capacidad de actuar sobre las excepciones, si lo desea. El comportamiento predeterminado todavía produce una excepción cuando se produce un error, coherente con otros proveedores de estado de sesión ASP.NET; el código existente debería funcionar igual que antes.

Si establece *throwOnError* a **false**, en lugar de producir una excepción cuando se produce un error, se producirá en modo silencioso. Para ver si se produjo un error y, si es así, detectar la excepción fue, compruebe la propiedad estática *Microsoft.Web.Redis.RedisSessionStateProvider.LastException*.

### <a name="notes-on-retrytimeoutinmilliseconds"></a>Notas sobre la *retryTimeoutInMilliseconds*

Esto proporciona alguna lógica de reintento para simplificar el caso de que alguna operación de sesión debe reintentar en caso de error debido a cosas como problema de red, mientras que también permite controlar el tiempo de espera de reintento o dejar de participar en reintento completamente.

Si establece *retryTimeoutInMilliseconds* en un número, por ejemplo, 2000, a continuación, cuando se produce un error en una operación de sesión, se volverá a intentar a 2000 milisegundos antes de tratarlo como un error. Por lo que para que el proveedor de estado de sesión para aplicar esta lógica de reintento, simplemente configure el tiempo de espera. El primer reintento se realizará después de 20 milisegundos, que es suficiente en la mayoría de los casos, cuando se produce un problema de red. Después de eso, volverá a intentar cada segundo hasta que se agota el tiempo. Justo después del tiempo de espera, volverá a intentar una vez más para asegurarse de que no servirá desactivar el tiempo de espera por un segundo (como máximo).

Si cree que no necesita volver a intentar (por ejemplo, cuando se está ejecutando el servidor de Redis en el mismo equipo que la aplicación) o si desea controlar la lógica de reintento personalmente, establezca *retryTimeoutInMilliseconds* en 0.

### <a name="about-redisserializertype"></a>About *redisSerializerType*

De forma predeterminada, se realiza la serialización para almacenar los valores en Redis en un formato binario proporcionado por el **BinaryFormatter** clase. Use *redisSerializerType* para especificar el nombre de tipo calificado de ensamblado de una clase que implementa **Microsoft.Web.Redis.ISerializer** y tiene la lógica personalizada para serializar y deserializar los valores. Por ejemplo, esta es una clase de serializador de Json con JSON.NET:

```cs
namespace MyCompany.Redis
{
    public class JsonSerializer : ISerializer
    {
        private static JsonSerializerSettings _settings = new JsonSerializerSettings() { TypeNameHandling = TypeNameHandling.All };

        public byte[] Serialize(object data)
        {
            return Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(data, _settings));
        }

        public object Deserialize(byte[] data)
        {
            if (data == null)6t6
            {
                return null;
            }
            return JsonConvert.DeserializeObject(Encoding.UTF8.GetString(data), _settings);
        }
    }
}
```

Suponiendo que esta clase se define en un ensamblado con nombre **MyCompanyDll**, puede establecer el parámetro *redisSerializerType* usarlo:

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvider"
             name = "MySessionStateStore"
             redisSerializerType = "MyCompany.Redis.JsonSerializer,MyCompanyDll"
             ... />
    </providers>
</sessionState>
```

## <a name="output-cache-directive"></a>Directiva de caché de salida

Incorpore una directiva OutputCache a cada página cuyos resultados desea almacenar en caché.

```xml
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

En el ejemplo anterior, los datos de la página almacenados en la memoria caché permanecerán ahí durante 60 segundos y se almacenará en la memoria caché una versión diferente de la página para cada combinación de parámetros. Para obtener más información sobre la directiva OutputCache, consulte [@OutputCache](https://go.microsoft.com/fwlink/?linkid=320837).

Después de realizar estos pasos, la aplicación está configurada para usar el proveedor de la caché de salida de Redis.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [proveedor de estado de sesión de ASP.NET para Azure Cache for Redis](cache-aspnet-session-state-provider.md).
