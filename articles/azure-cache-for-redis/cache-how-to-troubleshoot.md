---
title: Solución de problemas de Azure Cache for Redis| Microsoft Docs
description: Aprenda más sobre cómo resolver problemas comunes con Azure Cache for Redis.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/15/2019
ms.author: yegu
ms.openlocfilehash: 66361871d365068a90a2eeab70d92adb6b246a83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60830014"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Solución de problemas de Azure Cache for Redis

En este artículo se ayuda a solucionar problemas de diferentes categorías de incidencias que pueda experimentar al conectarse con las instancias de Azure Redis Cache.

- [La solución de problemas del lado cliente](#client-side-troubleshooting) ayuda a identificar y resolver incidencias en la aplicación que se conecta a la memoria caché.
- [La solución de problemas del lado servidor](#server-side-troubleshooting) ayuda a identificar y resolver incidencias que se producen del lado Azure Redis Cache.
- [La solución de problemas de pérdida de datos](#data-loss-troubleshooting) ayuda a identificar y resolver incidentes en los que se esperaban claves en la caché pero no se han encontrado.
- [Las excepciones de tiempo de expiración de StackExchange.Redis](#stackexchangeredis-timeout-exceptions) proporcionan información específica sobre cómo solucionar problemas con la biblioteca StackExchange.Redis.

> [!NOTE]
> Algunos de los pasos de solución de problemas de esta guía incluyen instrucciones para ejecutar comandos de Redis y supervisar diversas métricas de rendimiento. Para más información e instrucciones, consulte los artículos de la sección [Información adicional](#additional-information) .
>
>

## <a name="client-side-troubleshooting"></a>Solución de problemas de lado cliente

Esta sección describe problemas que se producen debido a una condición en la aplicación cliente.

- [Presión de memoria en el cliente](#memory-pressure-on-the-client)
- [Ráfagas de tráfico](#burst-of-traffic)
- [Uso elevado de la CPU del cliente](#high-client-cpu-usage)
- [Ancho de banda del lado cliente agotado](#client-side-bandwidth-exceeded)
- [Tamaño de solicitud/respuesta grande](#large-requestresponse-size)

### <a name="memory-pressure-on-the-client"></a>Presión de memoria en el cliente

La presión de memoria en la máquina del cliente genera todo tipo de problemas de rendimiento que pueden retrasar el procesamiento de respuestas de la caché. Cuando se alcanza la presión de memoria, el sistema puede paginar datos en el disco. Estos _errores de página_ hacen que el sistema se ralentice considerablemente.

Para detectar presión de memoria en el cliente:

- Supervise la utilización de memoria en la máquina para asegurarse de que no exceda la memoria disponible.
- Supervise el contador de rendimiento `Page Faults/Sec` del cliente. Durante el funcionamiento normal, la mayoría de los sistemas tienen algunos errores de página. Los picos de errores de página que corresponden con los tiempos de expiración de solicitudes pueden indicar la presión de memoria.

La presión de memoria alta en el cliente se puede mitigar varias maneras:

- Examine con más detalle los patrones de utilización de memoria para reducir el consumo de memoria en el cliente.
- Actualice la máquina virtual del cliente a un tamaño mayor con más memoria.

### <a name="burst-of-traffic"></a>Ráfagas de tráfico

Las ráfagas de tráfico se combinan con una mala configuración de `ThreadPool` pueden provocar retrasos en el procesamiento de datos ya enviados por el servidor Redis, pero que aún no se han consumido en el lado cliente.

Supervise cómo las estadísticas de `ThreadPool` cambian a lo largo del tiempo con [un ejemplo `ThreadPoolLogger`](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Puede usar mensajes de `TimeoutException` de StackExchange.Redis, tal como aparece debajo, para investigar más a fondo:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

En la excepción anterior, hay varias incidencias que son interesantes:

- Observe que en la sección `IOCP` y la sección `WORKER` tiene un valor de `Busy` que es mayor que el valor de `Min`. Esta diferencia significa que es necesario ajustar la configuración de `ThreadPool`.
- También puede ver `in: 64221`. Este valor indica que se han recibido 64 211 bytes en la capa de sockets del kernel del cliente, pero que la aplicación aún no los ha leído. Esta diferencia normalmente significa que la aplicación (por ejemplo, StackExchange.Redis) no está leyendo los datos de la red con la rapidez con la que el servidor se los envía.

Puede [configurar las opciones de `ThreadPool`](https://gist.github.com/JonCole/e65411214030f0d823cb) para asegurarse de que su grupo de subprocesos se escala verticalmente a gran velocidad en escenarios de ráfaga.

### <a name="high-client-cpu-usage"></a>Uso elevado de la CPU del cliente

Un uso elevado de la CPU del cliente indica que el sistema no puede seguir el ritmo del trabajo que se le ha solicitado hacer. Aunque la caché haya enviado la respuesta rápidamente, es posible que el cliente no pueda procesar la respuesta de manera oportuna.

Supervise la utilización de la CPU de todo el sistema del cliente mediante las métricas disponibles en Azure Portal o a través de los contadores de rendimiento en la máquina. Tenga cuidado de no supervisar la CPU de un *proceso*, porque un solo proceso puede tener un uso de CPU bajo pero la CPU de todo el sistema puede ser elevada. Busque picos de uso de CPU que correspondan a tiempos de espera agotados. Una utilización de la CPU alta también puede provocar valores de `in: XXX` en mensajes de error de `TimeoutException`, como se describe en la sección [Ráfagas de tráfico](#burst-of-traffic).

> [!NOTE]
> StackExchange.Redis 1.1.603 y versiones posteriores incluyen la métrica `local-cpu` en mensajes de error de `TimeoutException`. Asegúrese de usar la versión más reciente del [paquete StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Hay errores que se solucionan constantemente en el código que lo hacen más solido frente a tiempos de espera agotados, por lo que es importante tener la versión más reciente.
>
>

Para mitigar la utilización de la CPU elevada de un cliente:

- Investigue lo que está provocando los picos en la CPU.
- Actualice la máquina virtual del cliente a un tamaño mayor con más capacidad de CPU.

### <a name="client-side-bandwidth-exceeded"></a>Ancho de banda del lado cliente agotado

En función de la arquitectura de los equipos cliente, estos pueden tener limitaciones en el ancho de banda de red disponible. Si el cliente supera el ancho de banda disponible por una sobrecarga de la capacidad de red, los datos no se procesarán en el lado cliente con la rapidez con la que el servidor se los envía. Esta situación puede provocar tiempos de espera.

Supervise cómo cambia la utilización del ancho de banda a lo largo del tiempo mediante [un ejemplo `BandwidthLogger`](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Este código puede no ejecutarse correctamente en algunos entornos con permisos restringidos (como sitios web de Azure).

Para mitigarlo, reduzca el consumo de ancho de banda de red o aumente el tamaño de la máquina virtual del cliente a una con más capacidad de red.

### <a name="large-requestresponse-size"></a>Tamaño de solicitud/respuesta grande

Una solicitud/respuesta grande puede provocar tiempos de espera agotados. Por ejemplo, suponga que el valor del tiempo de expiración configurado en el cliente es de 1 segundo. La aplicación solicita dos claves (por ejemplo, "A" y "B") al mismo tiempo (con la utilización de la misma conexión de red física). La mayoría de clientes admiten la "canalización" de las solicitudes, en la que ambas solicitudes, "A" y "B", se envían una tras otra sin tener que esperar las respuestas. El servidor vuelve a enviar las respuestas en el mismo orden. Si la respuesta "A" es grande, puede consumir la mayor parte del tiempo de expiración para las solicitudes posteriores.

En el ejemplo siguiente, las solicitudes "A" y "B" se envían rápidamente al servidor. El servidor empieza a enviar las respuestas "A" y "B" rápidamente. Debido a los tiempos de transferencia de datos, la respuesta "B" debe esperar a que se agote el tiempo de espera de la respuesta "A", aunque el servidor haya respondido con rapidez.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Esta solicitud/respuesta es difícil de medir. Puede instrumentar el código del cliente para realizar un seguimiento de las respuestas y solicitudes grandes.

Las resoluciones para los tamaños grandes de respuesta varían, pero incluyen lo siguiente:

1. Optimización de la aplicación para un gran número de valores pequeños, en lugar de unos pocos valores grandes.
    - La mejor solución es dividir los datos en valores menores relacionados.
    - Consulte la publicación [What is the ideal value size range for redis? Is 100KB too large?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) (¿Cuál es el intervalo de tamaño de valor ideal para Redis? ¿100 KB es demasiado grande?) para obtener detalles de por qué se recomiendan valores más pequeños.
1. Aumento del tamaño de la máquina virtual para obtener mayores capacidades de ancho de banda.
    - Un aumento de ancho de banda en la máquina virtual del cliente o servidor puede reducir los tiempos de transferencia de datos para respuestas más grandes.
    - Compare la utilización actual de la red en ambas máquinas con los límites del tamaño actual de la máquina virtual. Es posible que obtener más ancho de banda solo en el servidor o en el cliente no sea suficiente.
1. Aumente el número de objetos de conexión que usa la aplicación.
    - Use un enfoque round robin para realizar solicitudes a través de objetos de conexión distintos.

## <a name="server-side-troubleshooting"></a>Solución de problemas del lado servidor

Esta sección describe problemas que se producen debido a una condición en el servidor de caché.

- [Presión de memoria en el servidor](#memory-pressure-on-the-server)
- [Uso elevado de la CPU/carga de servidor](#high-cpu-usage--server-load)
- [Ancho de banda del lado servidor agotado](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Presión de memoria en el servidor

La presión de memoria en el servidor genera todo tipo de problemas de rendimiento que pueden retrasar el procesamiento de solicitudes. Cuando se alcanza la presión de memoria, el sistema puede paginar datos en el disco. Estos _errores de página_ hacen que el sistema se ralentice considerablemente. Hay varias causas posibles de esta presión de memoria:

- La caché se rellena con los datos cerca de su capacidad máxima.
- Redis ve la fragmentación de memoria alta. El almacenamiento de objetos grandes es lo que suele provocar esta fragmentación, ya que Redis está optimizado para objetos pequeños.

Redis expone dos estadísticas a través del comando [INFO](https://redis.io/commands/info) que ayuda a identificar esta incidencia: "used_memory" y "used_memory_rss". También puede [ver estas métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor) mediante el portal.

Hay varios cambios posibles que puede hacer para ayudar a mantener una utilización de memoria correcta:

- [Configurar una directiva de memoria](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) y establecer tiempos de expiración de las claves. Esta directiva puede no ser suficiente si tiene fragmentación.
- [Configurar un valor rmaxmemory-reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) lo suficientemente grande como para compensar la fragmentación de la memoria. Para obtener más información, consulte debajo las [consideraciones para las reservas de memoria](#considerations-for-memory-reservations) adicionales.
- Dividir los objetos grandes que están en la memoria caché en objetos relacionados más pequeños.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la memoria usada para recibir una notificación anticipada sobre los impactos potenciales.
- [Escalar](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de memoria.

#### <a name="considerations-for-memory-reservations"></a>Consideraciones para las reservas de memoria

La actualización de valores de reserva de memoria, como maxmemory-reserved, puede afectar al rendimiento de la caché. Suponga que tiene una caché de 53 GB de los cuales se utilizan 49 GB de datos. Si se cambia el valor de reserva a 8 GB, la memoria máxima disponible del sistema se queda en 45 GB. Si los valores _used_memory_ o _used_memory_rss_ son superiores a 45 GB, el sistema puede expulsar datos hasta que _used_memory_ y _used_memory_rss_ estén por debajo de 45 GB. La expulsión puede aumentar la carga del servidor y la fragmentación de memoria.

### <a name="high-cpu-usage--server-load"></a>Uso elevado de la CPU/carga de servidor

Una carga o utilización de la CPU elevada en el servidor significa que el servidor no puede procesar solicitudes de manera oportuna. Puede que el servidor tarde en responder y sea capaz de seguir el ritmo con las tasas de solicitud.

[Supervise las métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor), como la carga de la CPU o el servidor. Busque picos de uso de CPU que correspondan a tiempos de espera agotados.

Hay varios cambios que puede hacer para mitigar la carga elevada en el servidor:

- Investigar qué está provocando los picos de la CPU, tales como la ejecución de [comandos caros](#expensive-commands) o los errores de página debido a la presión de memoria alta.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la carga de la CPU o el servidor para recibir una notificación anticipada sobre los impactos potenciales.
- [Escalar](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de CPU.

#### <a name="expensive-commands"></a>Comandos caros

No todos los comandos de Redis se crean por igual; algunos son más caros de ejecutar que otros. La [documentación de comandos de Redis](https://redis.io/commands) muestra la complejidad de tiempo de cada comando. Se recomienda revisar los comandos que se ejecutan en la caché para comprender el impacto en el rendimiento de esos comandos. Por ejemplo, el comando [KEYS](https://redis.io/commands/keys) a menudo se usa el sin saber que se trata de una operación O(N). Puede evitar el comando KEYS mediante el uso de [SCAN](https://redis.io/commands/scan) para reducir los picos de la CPU.

Mediante el comando [SLOWLOG](https://redis.io/commands/slowlog), puede medir los comandos caros que se ejecutan en el servidor.

### <a name="server-side-bandwidth-exceeded"></a>Ancho de banda del lado servidor superado

Tamaños de caché diferentes tienen capacidades distintas de ancho de banda de red. Si el servidor supera el ancho de banda disponible, los datos no se enviarán al cliente con la misma rapidez. Las solicitudes de los clientes podrían agotar el tiempo de espera debido a que el servidor no puede insertar datos al cliente lo suficientemente rápido.

Las métricas de "Lectura de caché" y "Escritura de caché" pueden usarse para ver el ancho de banda del lado servidor que se está usando. Puede [ver estas métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor) en el portal.

Para mitigar las situaciones en las que la utilización de ancho de banda de red está cerca de la capacidad máxima, haga lo siguiente:

- Cambiar el comportamiento de la llamada de cliente para reducir la demanda de la red.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la lectura de caché o la escritura de caché para recibir una notificación anticipada sobre los impactos potenciales.
- [Escalar](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de ancho de banda de red.

## <a name="data-loss-troubleshooting"></a>Solución de problemas de pérdida de datos

Esperaba que ciertos datos estuvieran en la instancia de Azure Cache for Redis, pero no parecen estar ahí.

Consulte [¿Qué ha ocurrido con mis datos en Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) para ver posibles causas y soluciones.

## <a name="stackexchangeredis-timeout-exceptions"></a>Excepciones de tiempo de espera de StackExchange.Redis

StackExchange.Redis usa una opción de configuración llamada `synctimeout` para operaciones sincrónicas, con un valor predeterminado de 1000 ms. Si no se completa una llamada sincrónica en este tiempo, el cliente de StackExchange.Redis genera un error de tiempo de expiración similar al ejemplo siguiente:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)

Este mensaje de error contiene métricas que pueden indicarle la causa y la posible solución del problema. La tabla siguiente contiene detalles acerca de las métricas del mensaje de error.

| Métrica del mensaje de error | Detalles |
| --- | --- |
| inst |En el último período de tiempo: se han emitido 0 comandos. |
| mgr |El administrador del socket está realizando `socket.select`, lo que significa que está solicitando al sistema operativo que indique un socket que tenga algo que hacer. El lector no está leyendo activamente de la red porque cree que no hay nada que hacer. |
| queue |Hay un total de 73 operaciones en curso. |
| qu |6 de las operaciones en curso están en la cola de no enviados y aún no se han escrito en la red de salida. |
| qs |67 de las operaciones en curso se han enviado al servidor, pero aún no hay una respuesta disponible. La respuesta podría ser `Not yet sent by the server` o `sent by the server but not yet processed by the client.`. |
| qc |0 de las operaciones en curso han visto respuestas, pero aún no se han marcado como completadas porque están esperando en el bucle de finalización. |
| wr |Hay un escritor activo (lo que significa que 6 solicitudes sin enviar no se omiten) bytes/activewriters. |
| bucear |No hay ningún lector activo y hay cero bytes disponibles para su lectura en los bytes/activereaders de NIC. |

### <a name="steps-to-investigate"></a>Pasos para investigar

1. Como procedimiento recomendado, asegúrese de usar el patrón siguiente para conectarse al usar el cliente de StackExchange.Redis.

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

    Para más información, consulte [Uso de Caché en Redis de Azure](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Asegúrese de que el servidor y la aplicación cliente están en la misma región de Azure. Por ejemplo, podría obtener tiempos de expiración cuando la caché está en Este de EE. UU., pero el cliente está en Oeste de EE. UU. y no se completa la solicitud en el intervalo `synctimeout`, o bien podría obtener tiempos de expiración cuando realiza la depuración en la máquina de desarrollo local. 

    Se recomienda encarecidamente que la memoria caché y el cliente estén en la misma región de Azure. Si tiene un escenario que incluye llamadas entre regiones, debe establecer el intervalo `synctimeout` a un valor mayor que el intervalo predeterminado de 1000 ms mediante la inclusión de una propiedad `synctimeout` en la cadena de conexión. En el ejemplo siguiente se muestra un fragmento de una cadena de conexión para StackExchange.Redis que proporciona Azure Redis Cache con un `synctimeout` de 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
1. Asegúrese de usar la versión más reciente del [paquete StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Hay errores que se solucionan constantemente en el código que lo hacen más solido frente a tiempos de espera agotados, por lo que es importante tener la versión más reciente.
1. Si las solicitudes se enlazan con las limitaciones de ancho de banda en el servidor o el cliente, tardan más en completarse y podrían provocar tiempos de expiración. Para ver si el tiempo de expiración se debe al ancho de banda de red del servidor, consulte [Ancho de banda del lado servidor superado](#server-side-bandwidth-exceeded). Para ver si el tiempo de expiración se debe al ancho de banda de red del cliente, consulte [Ancho de banda del lado cliente superado](#client-side-bandwidth-exceeded).
1. ¿la CPU está limitada en el servidor o en el cliente?

   - Compruebe si la CPU le está limitando en el cliente. Una CPU elevada podría provocar que la solicitud no se procese en el intervalo `synctimeout` y causar una solicitud para agotarse el tiempo de espera. Cambiar a un tamaño de cliente mayor o distribuir la carga puede ayudar a controlar este problema.
   - Supervise la `CPU` [métrica de rendimiento de caché](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) para comprobar si la CPU le está limitando en el servidor. Las solicitudes entrantes mientras Redis está limitado por la CPU pueden provocar que dichas solicitudes agoten el tiempo de espera. Para solucionar este problema, puede distribuir la carga entre varias particiones en una memoria caché premium o actualizar a un tamaño o plan de tarifa mayor. Para más información, consulte [Ancho de banda del lado servidor agotado](#server-side-bandwidth-exceeded).
1. ¿Hay comandos que tardan mucho tiempo en procesarse en el servidor? Los comandos de larga duración que tardan mucho tiempo en procesarse en el servidor de Redis pueden provocar tiempos de expiración. Para obtener más información acerca de los comandos de larga duración, consulte [comandos caros](#expensive-commands). Puede conectarse a la instancia de Azure Redis Cache mediante el cliente redis-cli o la [Consola de Redis](cache-configure.md#redis-console). Luego, ejecute el comando [SLOWLOG](https://redis.io/commands/slowlog) para ver si hay solicitudes más lentas de lo esperado. El servidor de Redis y StackExchange.Redis están optimizados para muchas solicitudes pequeñas, en lugar de menos solicitudes de gran tamaño. Dividir los datos en fragmentos menores puede mejorar las cosas aquí.

    Para obtener información acerca de cómo conectarse al punto de conexión SSL de la caché con redis-cli y stunnel, consulte La entrada de blog [Announcing ASP.NET Session State Provider for Redis Preview Release](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) (Anuncio de proveedor de estado de sesión ASP.NET para la versión preliminar de Redis).
1. Una carga alta del servidor de Redis puede causar tiempos de espera agotados. Puede supervisar la carga del servidor con la `Redis Server Load`[métrica de rendimiento de caché](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Una carga del servidor de 100 (valor máximo) significa que el servidor de redis ha estado ocupado procesando solicitudes, sin tiempo de inactividad. Para ver si ciertas solicitudes ocupan toda la funcionalidad del servidor, ejecute el comando SlowLog, como se describe en el párrafo anterior. Para más información, consulte Uso elevado de la CPU/carga de servidor.
1. ¿Ha habido cualquier otro evento en el lado cliente que puede haber causado una señalización visual de red? Los eventos comunes incluyen: escalado o reducción vertical del número de instancias de cliente, implementación de una nueva versión del cliente o escalado automático habilitado. En nuestra prueba hemos observado que el escalado automático o el escalado o reducción vertical pueden provocar que la conectividad de red de salida se pierda durante varios segundos. El código de StackExchange.Redis es resistente a dichos eventos y se vuelve a conectar. Mientras se produce la reconexión, las solicitudes en la cola pueden agotar el tiempo de espera.
1. ¿Ha habido una solicitud grande antes de varias solicitudes pequeñas en la caché que haya agotado el tiempo de espera? El parámetro `qs` en el mensaje de error indica cuántas solicitudes se enviaron del cliente al servidor pero que no han procesado una respuesta. Este valor puede seguir creciendo, ya que StackExchange.Redis usa una sola conexión de TCP y solo puede leer una respuesta cada vez. Aunque la primera operación ha agotado el tiempo de espera, esto no detiene el envío de más datos al servidor o desde este. Se bloquearán otras solicitudes hasta que la solicitud grande finalice y pueda provocar que se agote el tiempo de espera. Una solución es reducir la posibilidad de tiempos de espera agotados, garantizando que la memoria caché sea lo suficientemente grande para la carga de trabajo y dividiendo los valores grandes en fragmentos menores. Otra posible solución es utilizar un grupo de objetos `ConnectionMultiplexer` en el cliente y elegir el parámetro `ConnectionMultiplexer` con menos carga al enviar una solicitud nueva. La carga en varios objetos de conexión debería evitar que un único tiempo de expiración haga que otras solicitudes también agoten el tiempo de espera.
1. Si está utilizando `RedisSessionStateProvider`, asegúrese de haber configurado correctamente el tiempo de expiración de los reintentos. `retryTimeoutInMilliseconds` debe ser superior a `operationTimeoutInMilliseconds`; de lo contrario, no se producirá ningún reintento. En el siguiente ejemplo, `retryTimeoutInMilliseconds` se establece en 3000. Para más información, consulte [ASP.NET Session State Provider for Azure Cache for Redis](cache-aspnet-session-state-provider.md) (Proveedor de estado de sesión de ASP.NET para Azure Cache for Redis) y [How to use the configuration parameters of Session State Provider and Output Cache Provider](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration) (Uso de los parámetros de configuración del proveedor de estado de sesión y el proveedor de caché de resultados).

    ```xml
    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />
    ```

1. Compruebe el uso de la memoria en el servidor de Azure Cache for Redis mediante la [supervisión](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) de `Used Memory RSS` y `Used Memory`. Si hay una directiva de expulsión implementada, Redis empieza a expulsar las claves cuando `Used_Memory` alcanza el tamaño de la memoria caché. Idealmente, `Used Memory RSS` solo debe ser ligeramente mayor que `Used memory`. Una gran diferencia significa que hay fragmentación de memoria (interna o externa). Cuando `Used Memory RSS` es menor que `Used Memory`, significa que el sistema operativo ha intercambiado parte de la memoria caché. Si se produce este intercambio, puede esperar latencias importantes. Dado que Redis no tiene control sobre cómo se realizan sus asignaciones a las páginas, a menudo un parámetro `Used Memory RSS` elevado suele ser el resultado de un pico en la utilización de memoria. Cuando el servidor de Redis libera memoria, el asignador toma la memoria, pero puede devolver la memoria al sistema o no hacerlo. Puede haber una discrepancia entre el valor de `Used Memory` y el consumo de memoria comunicado por el sistema operativo. Es posible que Redis haya usado y liberado la memoria, pero no la haya devuelto al sistema. Puede realizar los pasos siguientes para ayudar a mitigar las incidencias de memoria:

   - Actualizar la caché a un tamaño mayor para que no se ejecute con limitaciones de memoria en el sistema.
   - Establecer tiempos de expiración de las claves para que los valores antiguos se expulsen de forma proactiva.
   - Supervisar la métrica de la memoria caché `used_memory_rss`. Cuando este valor se aproxima al tamaño de la caché, es probable que empiece a ver incidencias de rendimiento. Distribuir los datos entre varias particiones, si utiliza una caché premium, o actualizar a un tamaño de caché mayor.

   Para obtener más información, consulte [Presión de memoria en el servidor](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Información adicional

- [¿Qué oferta y tamaño de Azure Cache for Redis debo utilizar?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [¿Cómo se pueden realizar bancos de pruebas y probar el rendimiento del caché?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [¿Cómo puedo ejecutar comandos de Redis?](cache-faq.md#how-can-i-run-redis-commands)
- [Supervisión de Azure Cache for Redis](cache-how-to-monitor.md)
