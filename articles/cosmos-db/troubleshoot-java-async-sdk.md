---
title: Diagnosticar y solucionar problemas del SDK de Java Async de Azure Cosmos DB | Microsoft Docs
description: Use características como registro del lado cliente y otras herramientas de terceros para identificar, diagnosticar y solucionar problemas de Azure Cosmos DB.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: ee92a5dd474cdf4f32ed2c7327d732a2cfbbbf79
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632941"
---
# <a name="troubleshooting-issues-when-using-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Solución de problemas al usar el SDK de Java Async con las cuentas de la API de SQL de Azure Cosmos DB
En este artículo se tratan problemas comunes, soluciones alternativas, pasos de diagnóstico y herramientas al utilizar el [SDK de Java Async](sql-api-sdk-async-java.md) con las cuentas de la API de SQL de Azure Cosmos DB.
El SDK de Java Async proporciona la representación lógica del lado cliente para acceder a la API de SQL de Azure Cosmos DB. En este artículo se describen herramientas y enfoques para ayudarle si surge algún problema.

Comience con esta lista:
    * Eche un vistazo a la sección [Problemas comunes y soluciones alternativas] de este artículo.
    * Nuestro SDK es [de código abierto y está disponible en github](https://github.com/Azure/azure-cosmosdb-java) y supervisamos activamente la [sección de problemas](https://github.com/Azure/azure-cosmosdb-java/issues). Compruebe si encuentra algún problema similar con una solución alternativa ya registrada.
    * Revise las [sugerencias de rendimiento](performance-tips-async-java.md) y siga las prácticas sugeridas.
    * Siga el resto de este artículo, si no encuentra una solución, registre un [problema de GitHub](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Problemas comunes y soluciones alternativas

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Problemas de red, error de tiempo de espera de lectura de Netty, rendimiento bajo, latencia alta

#### <a name="general-suggestions"></a>Sugerencias generales
* Asegúrese de que la aplicación se está ejecutando en la misma región que la cuenta de Cosmos DB. 
* Compruebe el uso de la CPU en el host donde se ejecuta la aplicación. Si el uso de la CPU está en un 90 % o más, considere la posibilidad de ejecutar la aplicación en un host con una configuración mayor o distribuir la carga en varias máquinas.

#### <a name="connection-throttling"></a>Limitación de la conexión
La limitación de la conexión puede ocurrir por [Límite de conexiones en la máquina host] o por [agotamiento de puertos SNAT (PAT) de Azure]:

##### <a name="connection-limit-on-host"></a>Límite de conexiones en la máquina host
Algunos sistemas Linux (por ejemplo, "Red Hat") tienen un límite máximo para el número total de archivos abiertos. En Linux, los sockets se implementan como archivos, por lo que este número también limita el número total de conexiones.
Ejecute el siguiente comando:

```bash
ulimit -a
```
El número de archivos abiertos ("nofile") debe ser lo suficientemente grande (al menos el doble del tamaño del grupo de conexiones). Obtenga más información en las [sugerencias de rendimiento](performance-tips-async-java.md).

##### <a name="snat"></a>Agotamiento de puertos SNAT (PAT) de Azure

Si la aplicación está implementada en la máquina virtual de Azure sin una dirección IP pública, los [puertos SNAT de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) se utilizan por defecto para establecer conexiones con cualquier punto de conexión fuera de la máquina virtual. El número de conexiones permitidas desde la máquina virtual hasta el punto de conexión de Cosmos DB está limitado por la [configuración de Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

Los puertos SNAT de Azure se utilizan solo cuando la máquina virtual de Azure tiene una dirección IP privada y un proceso de la máquina virtual intenta establecer una conexión con una dirección IP pública. Por lo tanto, hay dos soluciones alternativas para evitar la limitación de Azure SNAT:
    * Agregue el punto de conexión del servicio de Azure Cosmos DB a la subred de la red virtual de la máquina virtual de Azure, como se explica en [Puntos de conexión del servicio de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). Cuando se habilita el punto de conexión de servicio, ya no se envían las solicitudes de una dirección IP pública a Cosmos DB; en su lugar, se envían la red virtual y la identidad de la subred. Este cambio puede producir caídas de firewall si solo se permiten direcciones IP públicas. Si usa un firewall, al habilitar el punto de conexión de servicio, agréguele una subred usando las [ACL de la red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
    * Asignar una dirección IP pública a la máquina virtual de Azure.

#### <a name="http-proxy"></a>Proxy HTTP

Si usa un Proxy HTTP, asegúrese de que pueda admitir el número de conexiones configuradas en el SDK de `ConnectionPolicy`.
En caso contrario, se encontrará con problemas de conexión.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Patrón de codificación no válido: bloquea el subproceso de E/S de Netty

El SDK usa la biblioteca de E/S de [Netty](https://netty.io/) para comunicarse con el servicio de Azure Cosmos DB. Contamos con Async API y usamos la API de E/S de Netty sin bloqueo. El trabajo de E/S del SDK se realiza en subprocesos de E/S de Netty. El número de subprocesos de E/S de Netty está configurado para ser el mismo que el número de núcleos de CPU de la máquina de la aplicación. Los subprocesos de E/S de Netty solo están diseñados para usarse con trabajos de E/S de Netty sin bloqueos. El SDK devuelve el resultado de la invocación de la API en uno de los subprocesos de E/S de Netty al código de la aplicación. Si la aplicación, después de recibir los resultados del subproceso de Netty, realiza una operación de larga duración en el subproceso de Netty, esto puede provocar que el SDK no tenga un número suficiente de subprocesos de E/S para realizar su trabajo de E/S interno. Este tipo de codificación de aplicación puede producir un rendimiento bajo, alta latencia y errores de `io.netty.handler.timeout.ReadTimeoutException`. La solución consiste en cambiar el subproceso cuando se sabe que la operación tardará tiempo.

   Por ejemplo, el siguiente fragmento de código muestra que si realiza un trabajo de larga duración, que tarde más de unos pocos milisegundos, en el subproceso de Netty, es posible que acabe en un estado donde no habrá ningún subproceso de E/S de Netty para procesar el trabajo de E/S y, como resultado, obtendrá ReadTimeoutException:
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // time consuming work. For example:
                        // writing to a file, computationally heavy work, or just sleep
                        // basically anything which takes more than a few milliseconds
                        // doing such operation on the IO netty thread
                        // without a proper scheduler, will cause problems.
                        // The subscriber will get ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //will be io.netty.handler.timeout.ReadTimeoutException
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   La solución consiste en cambiar el subproceso en el que realiza el trabajo que lleva tiempo. Defina una instancia singleton del Programador para la aplicación:
   ```java
// have a singleton instance of executor and scheduler
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Cuando necesite realizar un trabajo que lleve tiempo (por ejemplo, trabajo computacionalmente intensivo, E/S de bloqueo), cambie el subproceso a un trabajador que le proporcione `customScheduler` mediante `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // switches the thread.
        .subscribe(
            // ...
        );
```
Mediante el uso de `observeOn(customScheduler)`, se libera el subproceso de E/S de Netty y se cambia a su propio subproceso personalizado proporcionado por customScheduler. Esta modificación solucionará el problema y ya no recibirá el error `io.netty.handler.timeout.ReadTimeoutException`.

### <a name="connection-pool-exhausted-issue"></a>Problema de grupo de conexión agotado

`PoolExhaustedException` es un error del lado cliente. Si se produce a menudo este error, quiere decir que la carga de trabajo de la aplicación es mayor que lo que el grupo de conexiones del SDK puede servir. Es posible que aumentar el tamaño del grupo de conexiones o distribuir la carga en varias aplicaciones ayude.

### <a name="request-rate-too-large"></a>Tasa de solicitudes demasiado grande
Se trata de un error de servidor que indica que consumió el rendimiento aprovisionado y debe volver a intentarlo más tarde. Si recibe este error con frecuencia, considere la posibilidad de aumentar el rendimiento de la colección.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Error al conectarse al emulador de Azure Cosmos DB

El certificado HTTPS del emulador de Cosmos DB es un certificado autofirmado. Para que SDK funcione con el emulador, debe importar el certificado del emulador a Java TrustStore. Tal como se explica [aquí](local-emulator-export-ssl-certificates.md).


## <a name="enable-client-sice-logging"></a>Habilitar el registro del SDK de cliente

El SDK de Java Async usa SLF4j como fachada de registro que admite el registro en plataformas populares como log4j y logback.

Por ejemplo, si quiere usar log4j como plataforma de registro, agregue las siguientes bibliotecas en la classpath de Java:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Agregue también una configuración de log4j:
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Revise el [registro manual de sfl4j](https://www.slf4j.org/manual.html) para obtener más información.

## <a name="netstats"></a>Estadísticas de red de SO
Ejecute el comando netstat para hacerse una idea de cuántas conexiones se encuentran en el estado `Established`, en el estado `CLOSE_WAIT`, etc.

En Linux, puede ejecutar el comando siguiente:
```bash
netstat -nap
```
Filtrar el resultado para obtener solo las conexiones al punto de conexión de Cosmos DB.

Aparentemente, el número de conexiones al punto de conexión de Cosmos DB en el estado `Established` no debe ser mayor que el tamaño del grupo de conexiones configurado.

Si hay muchas conexiones al punto de conexión de Cosmos DB en el estado `CLOSE_WAIT`, como por ejemplo más de 1000, esto indica que las conexiones se establecen y cierran rápidamente, lo que podría causar problemas. Revise la sección [Problemas comunes y soluciones alternativas] para obtener más detalles.

 <!--Anchors-->
[Problemas comunes y soluciones alternativas]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Límite de conexiones en la máquina host]: #connection-limit-on-host
[Agotamiento de puertos SNAT (PAT) de Azure]: #snat


