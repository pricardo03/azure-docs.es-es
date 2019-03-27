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
ms.openlocfilehash: 838fc1da3e167d1df04fbb36a2fea33b8ac248a4
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482613"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Solución de problemas de Azure Cache for Redis

En este artículo le ayuda a solucionar problemas de las diferentes categorías de problemas que puede experimentar al conectar con Azure Cache para las instancias de Redis.

- [Solución de problemas de lado cliente](#client-side-troubleshooting) le ayuda a identificar y resolver problemas en la aplicación que se conecta a la memoria caché.
- [Solución de problemas de lado servidor](#server-side-troubleshooting) le ayuda a identificar y resolver los problemas que se producen en la memoria caché de Azure para Redis lado.
- [Solución de problemas de pérdida de datos](#data-loss-troubleshooting) le ayuda a identificar y resolver incidentes donde se esperaba pero no se encuentra en la memoria caché las claves.
- [Excepciones de tiempo de espera de StackExchange.Redis](#stackexchangeredis-timeout-exceptions) proporciona instrucciones específicas sobre cómo solucionar problemas con la biblioteca de StackExchange.Redis.

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

Presión de memoria en el equipo cliente genera todo tipo de problemas de rendimiento que pueden retrasar el procesamiento de respuestas de la memoria caché. Cuando se alcanza la presión de memoria, el sistema puede paginar datos en el disco. Estos _errores de página_ hacen que el sistema se ralentice considerablemente.

Para detectar la presión de memoria en el cliente:

- Supervisar el uso de memoria en el equipo para asegurarse de que no exceda la memoria disponible.
- Supervisar el cliente `Page Faults/Sec` contador de rendimiento. Durante el funcionamiento normal, la mayoría de los sistemas tiene algunos errores de página. Los picos de errores de página correspondiente con tiempos de espera de solicitud pueden indicar la presión de memoria.

Presión de memoria alta en el cliente se puede mitigar varias maneras:

- Profundice en los patrones de uso de memoria para reducir el consumo de memoria en el cliente.
- Actualizar al cliente de la máquina virtual a un tamaño mayor con más memoria.

### <a name="burst-of-traffic"></a>Ráfagas de tráfico

Las ráfagas de tráfico se combinan con una mala configuración de `ThreadPool` pueden provocar retrasos en el procesamiento de datos ya enviados por el servidor Redis, pero que aún no se han consumido en el lado cliente.

Monitor de cómo su `ThreadPool` estadísticas cambian con el tiempo mediante [un ejemplo `ThreadPoolLogger` ](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Puede usar `TimeoutException` mensajes de StackExchange.Redis como aparece a continuación investigar más a fondo:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

En la excepción anterior, hay varios problemas que son interesantes:

- Observe que en la sección `IOCP` y la sección `WORKER` tiene un valor de `Busy` que es mayor que el valor de `Min`. Esta diferencia significa su `ThreadPool` necesario ajustar la configuración.
- También puede ver `in: 64221`. Este valor indica que 64 211 bytes recibidos en la capa de sockets del kernel del cliente, pero no los ha leído por la aplicación. Esta diferencia normalmente significa que la aplicación (por ejemplo, StackExchange.Redis) no está leyendo datos de la red lo más rápido que el servidor se los envía a usted.

También puede [configurar su `ThreadPool` configuración](https://gist.github.com/JonCole/e65411214030f0d823cb) para asegurarse de que su grupo de subprocesos se escala verticalmente rápidamente en escenarios de ráfaga.

### <a name="high-client-cpu-usage"></a>Uso elevado de la CPU del cliente

Uso de CPU elevado de cliente indica que el sistema no puede mantener el trabajo que ya se haya formulado hacer. Aunque la memoria caché envíe la respuesta rápidamente, el cliente puede producir un error al procesar la respuesta de manera oportuna.

Supervisar el uso de CPU de todo el sistema del cliente mediante las métricas disponibles en Azure portal o a través de los contadores de rendimiento en el equipo. Tenga cuidado para no supervisar *proceso* CPU porque un solo proceso puede tener el uso de CPU bajo, pero la CPU de todo el sistema puede ser alta. Busque picos de uso de CPU que correspondan a tiempos de espera agotados. Elevado de CPU también puede causar gran `in: XXX` valores en `TimeoutException` mensajes de error como se describe en el [ráfagas de tráfico](#burst-of-traffic) sección.

> [!NOTE]
> StackExchange.Redis 1.1.603 y versiones posteriores incluyen la métrica `local-cpu` en mensajes de error de `TimeoutException`. Asegúrese de usar la versión más reciente del [paquete StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Hay errores que se solucionan constantemente en el código que lo hacen más solido frente a tiempos de espera agotados, por lo que es importante tener la versión más reciente.
>
>

Para mitigar el uso elevado de CPU de un cliente:

- Investigue qué está causando los picos de CPU.
- Actualice al cliente a un tamaño de máquina virtual con más capacidad de CPU.

### <a name="client-side-bandwidth-exceeded"></a>Ancho de banda del lado cliente agotado

En función de la arquitectura de los equipos cliente, estos pueden tener limitaciones en el ancho de banda de red disponible. Si el cliente supera el ancho de banda disponible mediante la sobrecarga de la capacidad de red, a continuación, datos no se procesan en el lado cliente tan pronto como el servidor se los envía. Esta situación puede provocar tiempos de espera.

Supervisar cómo el uso de ancho de banda cambian con el tiempo mediante [un ejemplo `BandwidthLogger` ](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Este código puede no ejecutarse correctamente en algunos entornos con permisos restringidos (como sitios web de Azure).

Para mitigar, reducir el consumo de ancho de banda de red o aumente el tamaño de máquina virtual con más capacidad de red de cliente.

### <a name="large-requestresponse-size"></a>Tamaño de solicitud/respuesta grande

Una solicitud/respuesta grande puede provocar tiempos de espera agotados. Por ejemplo, suponga que el valor de tiempo de espera configurado en el cliente es de 1 segundo. La aplicación solicita dos claves (por ejemplo, "A" y "B") al mismo tiempo (con la utilización de la misma conexión de red física). La mayoría de los clientes admiten la solicitud "canalización", donde ambas solicitudes 'A' y 'B' se envían una tras otra sin tener que esperar sus respuestas. El servidor vuelve a enviar las respuestas en el mismo orden. Si la respuesta 'A' es grande, puede consumir la mayor parte del tiempo de espera para las solicitudes posteriores.

En el ejemplo siguiente, la solicitud 'A' y 'B' se envían rápidamente en el servidor. El servidor comienza a enviar las respuestas 'A' y 'B' rápidamente. Debido a los tiempos de transferencia de datos, que debe esperar detrás de respuesta 'A' a 'B' tiempos de respuesta, aunque el servidor respondió con rapidez.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Esta solicitud/respuesta es difícil de medir. Puede instrumentar el código de cliente para realizar un seguimiento de las respuestas y solicitudes de gran tamaño.

Resoluciones para tamaños de respuesta grande varían, pero incluyen:

1. Optimizar la aplicación para un gran número de valores pequeños, en lugar de unos pocos valores grandes.
    - La mejor solución es dividir los datos en valores menores relacionados.
    - ¿Vea la entrada de blog [cuál es el intervalo de tamaño de valor ideal para redis? ¿100 KB es demasiado grande? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) para obtener más información sobre por qué se recomiendan valores más pequeños.
1. Aumentar el tamaño de la máquina virtual para obtener mayores capacidades de ancho de banda
    - Más ancho de banda en el cliente o la máquina virtual del servidor puede reducir los tiempos de transferencia de datos para respuestas mayores.
    - Compare el uso actual de la red en ambos equipos a los límites de su tamaño actual de la máquina virtual. Más ancho de banda solo en el servidor o solo en el cliente puede no ser suficiente.
1. Aumentar el número de objetos de conexión que usa la aplicación.
    - Utilice un enfoque round-robin para realizar solicitudes a través de los objetos de conexión diferentes.

## <a name="server-side-troubleshooting"></a>Solución de problemas de lado servidor

Esta sección describe problemas que se producen debido a una condición en el servidor de caché.

- [Presión de memoria en el servidor](#memory-pressure-on-the-server)
- [Uso elevado de la CPU/carga de servidor](#high-cpu-usage--server-load)
- [Ancho de banda del lado servidor agotado](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Presión de memoria en el servidor

La presión de memoria en el servidor genera todo tipo de problemas de rendimiento que pueden retrasar el procesamiento de solicitudes. Cuando se alcanza la presión de memoria, el sistema puede paginar datos en el disco. Estos _errores de página_ hacen que el sistema se ralentice considerablemente. Hay varias causas posibles de esta presión de memoria:

- La memoria caché se rellena con los datos cerca de su capacidad máxima.
- Redis está viendo la fragmentación de memoria alta. Esta fragmentación suelen deberse almacenar objetos grandes, ya que Redis está optimizado para objetos pequeños.

Redis expone dos estadísticas a través de la [información](https://redis.io/commands/info) comandos que pueden ayudar a identificar este problema: "used_memory" y "used_memory_rss". También puede [ver estas métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor) mediante el portal.

Hay varios cambios posibles que puede hacer para ayudar a mantener el uso de memoria correcto:

- [Configurar una directiva de memoria](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) y establecer tiempos de expiración de las claves. Esta directiva puede no ser suficiente si tiene fragmentación.
- [Configurar un valor rmaxmemory-reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) lo suficientemente grande como para compensar la fragmentación de la memoria. Para obtener más información, vea el adicionales [consideraciones para las reservas de memoria](#considerations-for-memory-reservations) a continuación.
- Dividir los objetos grandes que están en la memoria caché en objetos relacionados más pequeños.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la memoria usada para recibir una notificación al principio sobre los impactos potenciales.
- [Escala](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de memoria.

#### <a name="considerations-for-memory-reservations"></a>Consideraciones para las reservas de memoria

Actualizar valores de reserva de memoria, como maxmemory-reserved, puede afectar al rendimiento de la caché. Suponga que tiene una memoria caché de 53 GB que se rellena con 49 GB de datos. Cambiar el valor de reserva a 8 GB, quita memoria máxima de disponible del sistema a 45 GB. Si _used_memory_ o _used_memory_rss_ valores son superiores a 45 GB, el sistema puede expulsar datos hasta que ambos _used_memory_ y _used_memory_rss_ están por debajo de 45 GB. La expulsión puede aumentar la carga del servidor y la fragmentación de memoria.

### <a name="high-cpu-usage--server-load"></a>Uso elevado de la CPU/carga de servidor

Una carga elevada en el servidor o el uso de CPU significa que el servidor no puede procesar solicitudes de manera oportuna. El servidor puede ser lento para responder y no se puede mantenerse al día con las tasas de solicitud.

[Supervisar las métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor) como la carga de CPU o de servidor. Busque picos de uso de CPU que correspondan a tiempos de espera agotados.

Hay varios cambios que puede hacer para mitigar la carga elevada en el servidor:

- Investigue qué está causando los picos de CPU como ejecución [comandos costosos](#expensive-commands) o página que se produzca un error debido a la presión de memoria alta.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la carga de CPU o de servidor que se notifique al principio los impactos potenciales.
- [Escala](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de CPU.

#### <a name="expensive-commands"></a>Comandos costos

No todos los comandos de Redis se crean por igual, algunas son más costosas en comparación con otros usuarios. El [comandos de Redis documentación](https://redis.io/commands) muestra la complejidad de tiempo de cada comando. Se recomienda que revisar los comandos que se ejecutan en la memoria caché para comprender el impacto de rendimiento de esos comandos. Por ejemplo, el [claves](https://redis.io/commands/keys) a menudo se usa el comando sin saber que se trata de una operación o (n). Puede evitar las claves mediante [SCAN](https://redis.io/commands/scan) reducir la CPU los picos.

Mediante el [SLOWLOG](https://redis.io/commands/slowlog) de comandos, puede medir costosos comandos que se ejecute en el servidor.

### <a name="server-side-bandwidth-exceeded"></a>Ancho de banda del lado servidor agotado

Tamaños de caché diferentes tienen capacidades de ancho de banda de red diferente. Si el servidor supera el ancho de banda disponible, no se enviará al cliente como rápidamente datos. Las solicitudes de clientes podrían tiempo de espera porque el servidor no puede insertar datos en el cliente lo suficientemente rápido.

Las métricas de "Lectura" y "Escritura de caché" pueden utilizarse para ver cuánto ancho de banda del lado servidor se está usando. También puede [ver estas métricas](cache-how-to-monitor.md#view-metrics-with-azure-monitor) en el portal.

Para mitigar las situaciones donde es el uso de ancho de banda de red cerca de la capacidad máxima:

- Cambiar el comportamiento de llamada de cliente para reducir la demanda de la red.
- [Crear alertas](cache-how-to-monitor.md#alerts) en métricas como la lectura de caché o de escritura de caché que se notifique al principio los impactos potenciales.
- [Escala](cache-how-to-scale.md) a un tamaño mayor de caché con más capacidad de ancho de banda de red.

## <a name="data-loss-troubleshooting"></a>Solución de problemas de pérdida de datos

Esperaba que ciertos datos estuvieran en la instancia de Azure Cache for Redis, pero no parecen estar ahí.

Consulte [¿Qué ha ocurrido con mis datos en Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) para ver posibles causas y soluciones.

## <a name="stackexchangeredis-timeout-exceptions"></a>Excepciones de tiempo de espera de StackExchange.Redis

StackExchange.Redis usa la opción de configuración `synctimeout` para operaciones sincrónicas y su valor predeterminado de 1000 ms. Si no se completa una llamada sincrónica en este momento, el cliente de StackExchange.Redis genera un error de tiempo de espera similar al ejemplo siguiente:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)

Este mensaje de error contiene métricas que pueden indicarle la causa y la posible solución del problema. La tabla siguiente contiene detalles acerca de las métricas del mensaje de error.

| Métrica del mensaje de error | Detalles |
| --- | --- |
| inst |En el último período de tiempo: se han emitido 0 comandos. |
| mgr |El administrador del socket está realizando `socket.select`, lo que significa que está solicitando al sistema operativo que indique un socket que tiene algo que hacer. El lector no está leyendo activamente de la red porque cree que no hay nada que ver |
| queue |Hay un total de 73 operaciones en curso. |
| qu |6 de las operaciones en curso en la cola sin enviar y aún no se han escrito en la red de salida |
| qs |67 de las operaciones en curso se enviaron al servidor, pero aún no está disponible una respuesta. La respuesta podría ser `Not yet sent by the server` o `sent by the server but not yet processed by the client.`. |
| qc |0 de las operaciones en curso han visto respuestas, pero aún no se han marcado como completa porque está esperando en el bucle de finalización. |
| wr |Hay un bytes de escritor activo (es decir, que no se ha omitido el 6 solicitudes sin enviar) / activewriters. |
| bucear |No hay ningún lector activo y hay cero bytes disponibles para su lectura en los bytes/activereaders de NIC. |

### <a name="steps-to-investigate"></a>Pasos para investigar

1. Como práctica recomendada, asegúrese de que está utilizando el modelo siguiente para conectarse al usar al cliente StackExchange.Redis.

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

1. Asegúrese de que el servidor y la aplicación cliente están en la misma región de Azure. Por ejemplo, podría obtener tiempos de espera cuando la memoria caché está en este de Estados Unidos, pero el cliente está en oeste de Estados Unidos y la solicitud no se completa dentro del `synctimeout` intervalo, o bien podría obtener tiempos de espera cuando se depura desde el equipo de desarrollo local. 

    Se recomienda encarecidamente que la memoria caché y el cliente estén en la misma región de Azure. Si tiene un escenario que incluye llamadas entre regiones, debe establecer el intervalo `synctimeout` a un valor mayor que el intervalo predeterminado de 1000 ms mediante la inclusión de una propiedad `synctimeout` en la cadena de conexión. En el ejemplo siguiente se muestra un fragmento de código de una cadena de conexión para StackExchange.Redis proporcionadas por la memoria caché de Azure para Redis con una `synctimeout` de 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
1. Asegúrese de usar la versión más reciente del [paquete StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Hay errores que se solucionan constantemente en el código que lo hacen más solido frente a tiempos de espera agotados, por lo que es importante tener la versión más reciente.
1. Si las solicitudes se enlazan las limitaciones de ancho de banda en el servidor o cliente, tarda más tiempo para que puedan completarse y puede provocar tiempos de espera. Para ver si es el tiempo de espera debido a ancho de banda de red en el servidor, consulte [ancho de banda del lado servidor agotado](#server-side-bandwidth-exceeded). Para ver si es el tiempo de espera debido a ancho de banda de red de cliente, consulte [ancho de banda del lado cliente agotado](#client-side-bandwidth-exceeded).
1. ¿la CPU está limitada en el servidor o en el cliente?

   - Compruebe si está limitando CPU en el cliente. Uso de CPU elevado podría provocar que la solicitud no se pueda procesar en el `synctimeout` intervalo y causa una solicitud de tiempo de espera. Cambiar a un tamaño de cliente mayor o distribuir la carga puede ayudar a controlar este problema.
   - Compruebe si le esté sacando CPU enlazada al servidor mediante la supervisión de la `CPU` [métrica de rendimiento de caché](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Las solicitudes entrantes mientras Redis está limitado por la CPU pueden provocar que dichas solicitudes en tiempo de espera. Para solucionar este problema, puede distribuir la carga entre varias particiones en una memoria caché premium o actualizar a un tamaño o plan de tarifa mayor. Para más información, consulte [Ancho de banda del lado servidor agotado](#server-side-bandwidth-exceeded).
1. ¿Hay comandos que tardan mucho tiempo en procesarse en el servidor? Comandos de ejecución prolongada que tardan mucho tiempo en procesarse en el servidor de redis pueden provocar tiempos de espera. Para obtener más información acerca de los comandos de ejecución prolongada, vea [comandos costosos](#expensive-commands). Puede conectarse a la caché de Azure para la instancia de Redis mediante el cliente de redis-cli o la [consola de Redis](cache-configure.md#redis-console). A continuación, ejecute el [SLOWLOG](https://redis.io/commands/slowlog) comando para ver si hay solicitudes más lentamente de lo esperado. El servidor de Redis y StackExchange.Redis están optimizados para muchas solicitudes pequeñas, en lugar de menos solicitudes de gran tamaño. Dividir los datos en fragmentos menores puede mejorar las cosas aquí.

    Para obtener información acerca de cómo conectarse al punto de conexión de la caché SSL con redis-cli y stunnel, consulte la entrada de blog [Announcing ASP.NET Session State Provider para la versión de vista previa de Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).
1. Una carga alta del servidor de Redis puede causar tiempos de espera agotados. Puede supervisar la carga del servidor con la `Redis Server Load`[métrica de rendimiento de caché](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Una carga del servidor de 100 (valor máximo) significa que el servidor de redis ha estado ocupado procesando solicitudes, sin tiempo de inactividad. Para ver si ciertas solicitudes ocupan toda la funcionalidad del servidor, ejecute el comando SlowLog, como se describe en el párrafo anterior. Para más información, consulte Uso elevado de la CPU/carga de servidor.
1. ¿Ha habido cualquier otro evento en el lado cliente que puede haber causado una señalización visual de red? Los eventos comunes incluyen: escalar el número de instancias de cliente o hacia abajo, implementa una nueva versión del cliente o el escalado automático habilitado. En nuestras pruebas hemos encontrado que escalado automático o escalar arriba/abajo puede hacer que la conectividad de red saliente que se pierdan durante varios segundos. El código de StackExchange.Redis es resistente a dichos eventos y se vuelve a conectar. Al volver a conectar, las solicitudes en la cola pueden agotar el tiempo.
1. ¿Ha habido una solicitud grande anterior varias solicitudes pequeñas en la memoria caché que se agotó el tiempo? El parámetro `qs` en el error mensaje indica cuántas solicitudes se enviaron desde el cliente al servidor, pero aún no ha procesado una respuesta. Este valor puede seguir creciendo, ya que StackExchange.Redis usa una sola conexión de TCP y solo puede leer una respuesta cada vez. Aunque la primera operación ha agotado el tiempo de espera, no se detiene más datos que se envíen a o desde el servidor. Las demás solicitudes se bloquearán hasta que finalice la solicitud grande y puede provocar tiempos de espera. Una solución es reducir la posibilidad de tiempos de espera agotados, garantizando que la memoria caché sea lo suficientemente grande para la carga de trabajo y dividiendo los valores grandes en fragmentos menores. Otra posible solución es utilizar un grupo de objetos `ConnectionMultiplexer` en el cliente y elegir el parámetro `ConnectionMultiplexer` con menos carga al enviar una solicitud nueva. La carga entre varios objetos de conexión debe impedir que un único tiempo de espera haga que otras solicitudes también el tiempo de espera.
1. Si usa `RedisSessionStateProvider`, asegúrese de que ha establecido correctamente el tiempo de espera de reintento. `retryTimeoutInMilliseconds` debe ser superior a `operationTimeoutInMilliseconds`; de lo contrario, no se producirá ningún reintento. En el siguiente ejemplo, `retryTimeoutInMilliseconds` se establece en 3000. Para más información, consulte [ASP.NET Session State Provider for Azure Cache for Redis](cache-aspnet-session-state-provider.md) (Proveedor de estado de sesión de ASP.NET para Azure Cache for Redis) y [How to use the configuration parameters of Session State Provider and Output Cache Provider](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration) (Uso de los parámetros de configuración del proveedor de estado de sesión y el proveedor de caché de resultados).

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

1. Compruebe el uso de la memoria en el servidor de Azure Cache for Redis mediante la [supervisión](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) de `Used Memory RSS` y `Used Memory`. Si hay una directiva de expulsión implementada, Redis empieza a expulsar las claves cuando `Used_Memory` alcanza el tamaño de la memoria caché. Idealmente, `Used Memory RSS` solo debe ser ligeramente mayor que `Used memory`. Una gran diferencia significa que no hay fragmentación de memoria (internos o externos). Cuando `Used Memory RSS` es menor que `Used Memory`, significa que el sistema operativo ha intercambiado parte de la memoria caché. Si se produce este intercambio, puede esperar latencias importantes. Dado que Redis no tiene control sobre cómo se asignan sus asignaciones a páginas en memoria, alta `Used Memory RSS` suele ser el resultado de un pico de uso de memoria. Cuando el servidor de Redis libera la memoria, el asignador de toma de la memoria pero puede o puede no devolver la memoria al sistema. Puede haber una discrepancia entre el valor de `Used Memory` y el consumo de memoria comunicado por el sistema operativo. Memoria puede haberse utilizada y publicada por Redis pero no al sistema. Para ayudar a mitigar los problemas de memoria, puede hacer lo siguiente:

   - Actualizar la memoria caché a un tamaño mayor para que no se estén ejecutando con las limitaciones de memoria en el sistema.
   - Establecer tiempos de expiración de las claves para que los valores antiguos se expulsen de forma proactiva.
   - Supervisar la métrica de la memoria caché `used_memory_rss`. Cuando este valor aproxima al tamaño de la memoria caché, es probable que empezar a ver los problemas de rendimiento. Distribuir los datos entre varias particiones si utiliza una caché premium, o actualizar a un tamaño de caché mayor.

   Para obtener más información, consulte [Presión de memoria en el servidor](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Información adicional

- [¿Qué oferta y tamaño de Azure Cache for Redis debo utilizar?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [¿Cómo se pueden realizar bancos de pruebas y probar el rendimiento del caché?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [¿Cómo puedo ejecutar comandos de Redis?](cache-faq.md#how-can-i-run-redis-commands)
- [Supervisión de Azure Cache for Redis](cache-how-to-monitor.md)
