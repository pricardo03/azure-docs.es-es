---
title: Guía de rendimiento para Azure SignalR Service
description: Información general sobre el rendimiento de Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: f7cc05c8c2a299d809c4386d119fef58fa2548d5
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59579247"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Guía de rendimiento para Azure SignalR Service

Una de las principales ventajas de usar Azure SignalR Service es la facilidad de escalado de aplicaciones de SignalR. En un escenario a gran escala, el rendimiento es un factor importante. 

En esta guía, presentaremos los factores que afectan al rendimiento de la aplicación de SignalR. Vamos a describir típicos en el rendimiento en escenarios de casos de uso diferentes. Al final, presentaremos el entorno y herramientas que puede usar para generar un informe de rendimiento.

## <a name="term-definitions"></a>Definiciones de términos

*Entrada*: El mensaje entrante a Azure SignalR Service.

*Salida*: El mensaje saliente de Azure SignalR Service.

*Ancho de banda*: El tamaño total de todos los mensajes de 1 segundo.

*Modo predeterminado*: El modo de funcionamiento predeterminado cuando se crea una instancia de Azure SignalR Service. Azure SignalR Service espera que el servidor de aplicaciones para establecer una conexión con él antes de acepte las conexiones de cliente.

*Modo sin servidor*: Modo en que Azure SignalR Service acepta únicamente conexiones de cliente. No se permite ninguna conexión de servidor.

## <a name="overview"></a>Información general

Azure SignalR Service define siete niveles estándar para las capacidades de rendimiento diferente. Esta guía responde las preguntas siguientes:

-   ¿Qué es el rendimiento de Azure SignalR Service típico para cada nivel?

-   ¿Azure SignalR Service cumple Mis requisitos de rendimiento de mensajes (por ejemplo, envío de mensajes 100 000 por segundo)?

-   ¿Para mi escenario concreto, qué nivel es adecuado para mí? O bien, ¿cómo puedo seleccionar el nivel adecuado?

-   ¿Qué tipo de servidor de aplicaciones (tamaño de máquina virtual) es adecuada para mí? ¿Cuántos de ellos debo implementar?

Para responder a estas preguntas, esta guía primero proporciona una explicación detallada de los factores que afectan al rendimiento. A continuación, se muestra el número máximo de mensajes entrante y saliente para cada nivel para casos de uso típicos: **echo**, **difusión**, **envío al grupo**, y **enviar a conexión** (peer-to-peer conversando).

Esta guía no puede cubrir todos los escenarios (y patrones de envío de mensajes distintos casos de uso, los tamaños de mensaje y así sucesivamente). Sino que proporciona algunos métodos para ayudarle:

- Evaluar los requisitos aproximados de los mensajes entrantes o salientes.
- Encuentre los niveles adecuados mediante la comprobación de la tabla de rendimiento.

## <a name="performance-insight"></a>Información de rendimiento

En esta sección se describe las metodologías de evaluación del rendimiento y, a continuación, se enumera todos los factores que afectan al rendimiento. Al final, proporciona métodos para ayudarle a evaluar los requisitos de rendimiento.

### <a name="methodology"></a>Metodología

*Rendimiento* y *latencia* típico consta de la comprobación de rendimiento. Para Azure SignalR Service, cada nivel SKU tiene su propia directiva de limitación de rendimiento. Define la directiva *el máximo permitido de rendimiento (ancho de banda entrante y saliente)* como el máximo rendimiento obtenido al 99 por ciento de los mensajes tienen una latencia que es menor que 1 segundo.

La latencia es el intervalo de tiempo de la conexión que envía el mensaje para recibir el mensaje de respuesta de Azure SignalR Service. Vamos a echar **echo** como ejemplo. Cada conexión de cliente, agrega una marca de tiempo en el mensaje. Concentrador del servidor de la aplicación envía el mensaje original al cliente. Por lo tanto, el retraso de propagación fácilmente se calcula por cada conexión de cliente. La marca de tiempo se adjunta para todos los mensajes de **difusión**, **envío al grupo**, y **enviar a conexión**.

Para simular miles de conexiones de cliente simultáneas, se crean varias máquinas virtuales en una red privada virtual en Azure. Todas estas máquinas virtuales se conectarán a la misma instancia de Azure SignalR Service.

En el modo predeterminado de Azure SignalR Service, las máquinas virtuales del servidor de aplicaciones se implementan en la misma red privada virtual como máquinas virtuales del cliente. Todas las máquinas virtuales y las máquinas virtuales del servidor de aplicaciones se implementan en la misma red de la misma región para evitar la latencia entre regiones.

### <a name="performance-factors"></a>Factores de rendimiento

En teoría, capacidad de Azure SignalR Service está limitada por los recursos de cálculo: CPU, memoria y red. Por ejemplo, más conexiones a Azure SignalR Service que el servicio use más memoria. Para mayor tráfico de mensajes (por ejemplo, todos los mensajes es mayor que 2048 bytes), debe dedicar más ciclos de CPU para procesar el tráfico de Azure SignalR Service. Mientras tanto, el ancho de banda de red de Azure también impone un límite para el tráfico máximo.

El tipo de transporte es otro factor que afecta al rendimiento. Los tres tipos son [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [evento enviado del servidor](https://en.wikipedia.org/wiki/Server-sent_events), y [sondeo largo](https://en.wikipedia.org/wiki/Push_technology). 

WebSocket es un protocolo de comunicación dúplex completo y bidireccional a través de una única conexión TCP. Evento enviado del servidor es un protocolo unidireccional para insertar mensajes del servidor al cliente. Sondeo largo requiere que los clientes sondear periódicamente la información del servidor a través de una solicitud HTTP. Para la misma API en las mismas condiciones, WebSocket tiene el mejor rendimiento, eventos enviados del servidor es más lento y sondeo largo es el más lento. Azure SignalR Service, se recomienda WebSocket de forma predeterminada.

El costo de enrutamiento de mensajes también limita el rendimiento. Azure SignalR Service desempeña un papel como un enrutador de mensajes, que enruta el mensaje de un conjunto de clientes o servidores a otros clientes o servidores. Un escenario diferente o la API requiere una directiva de enrutamiento distintos. 

Para **echo**, el cliente envía un mensaje a sí mismo y el destino del enrutamiento es en sí mismo. Este patrón tiene el menor costo de enrutamiento. Pero para **difusión**, **envío al grupo**, y **enviar a conexión**, Azure SignalR Service debe buscar las conexiones de destino a través de los datos internos distribuidos estructura. Este procesamiento adicional usa más CPU, memoria y ancho de banda de red. Como resultado, el rendimiento es más lento.

En el modo predeterminado, el servidor de la aplicación también podría convertirse en un cuello de botella para determinados escenarios. El SDK de Azure SignalR tiene que invocar el concentrador, al tiempo que mantiene una conexión dinámica con todos los clientes a través de las señales de latido.

En el modo sin servidor, el cliente envía un mensaje mediante HTTP post, que no es tan eficaz como WebSocket.

Otro factor es el protocolo: JSON y [MessagePack](https://msgpack.org/index.html). MessagePack es un tamaño reducido y entregado con más rapidez que JSON. MessagePack no puede mejorar el rendimiento, sin embargo. El rendimiento de Azure SignalR Service no es sensible a protocolos porque la carga del mensaje que no descodificar durante el reenvío de mensajes de los clientes a servidores o viceversa.

En resumen, los siguientes factores afectan a la capacidad de entrada y salida:

-   Nivel de SKU (CPU y memoria)

-   Número de conexiones

-   Tamaño del mensaje

-   tasa de envío de mensajes

-   Tipo de transporte (WebSocket, evento enviado del servidor o el sondeo largo)

-   Escenario de caso de uso (costo de enrutamiento)

-   conexiones de servidor y el servicio de aplicación (en modo de servidor)


### <a name="finding-a-proper-sku"></a>Encontrar una SKU adecuada

¿Cómo puede evaluar la capacidad de entrada/salida o buscar qué nivel es adecuado para un caso de uso específico?

Se supone que el servidor de aplicación es lo suficientemente eficaz y no es el cuello de botella de rendimiento. A continuación, compruebe el máximo ancho de banda entrante y saliente para cada nivel.

#### <a name="quick-evaluation"></a>Evaluación rápida

Vamos a simplificar la evaluación en primer lugar, suponiendo algunos valores predeterminados: 

- El tipo de transporte es WebSocket.
- El tamaño del mensaje es de 2.048 bytes.
- Se envía un mensaje cada segundo.
- Azure SignalR Service está en el modo predeterminado.

Cada nivel tiene su propio máximo ancho de banda entrante y saliente. No se garantiza una experiencia de usuario después de la conexión de entrada o salida supera el límite.

**Echo** ofrece el máximo ancho de banda entrante porque tiene el menor costo de enrutamiento. **Difusión** define el ancho de banda máximo del mensaje saliente.

Hacer *no* superan los valores resaltados en las dos tablas siguientes.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| **Ancho de banda entrante** | **2 MBps**    | **4 MBps**    | **10 MBps**   | **20 MBps**    | **40 MBps**    | **100 MBps**   | **200 MBps**    |
| Ancho de banda saliente | 2 MBps   | 4 MBps   | 10 MBps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |


|     Difusión             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Ancho de banda entrante  | 4 kBps   | 4 kBps   | 4 kBps    | 4 kBps    | 4 kBps    | 4 kBps     | 4 kBps    |
| **Ancho de banda saliente** | **4 MBps**    | **8 MBps**    | **20 MBps**    | **40 MBps**    | **80 MBps**    | **200 MBps**    | **400 MBps**   |

*Ancho de banda de entrada* y *ancho de banda saliente* tienen el tamaño total de mensajes por segundo.  Estas son las fórmulas para ellos:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

- *inboundConnections*: El número de conexiones y enviará el mensaje.

- *outboundConnections*: El número de conexiones que recibe el mensaje.

- *messageSize*: El tamaño de un único mensaje (valor medio). Un mensaje pequeño de menos de 1.024 bytes tiene un impacto sobre el rendimiento es similar a un mensaje de 1.024 bytes.

- *sendInterval*: La hora de enviar un mensaje. Normalmente es 1 segundo por cada mensaje, lo que implica enviar un mensaje cada segundo. Un intervalo más pequeño significa enviar más mensajes en un período de tiempo. Por ejemplo, 0,5 segundos por cada mensaje significa enviar dos mensajes cada segundo.

- *Conexiones*: El umbral máximo confirmado de Azure SignalR Service para cada nivel. Si el número de conexiones aumenta aún más, se verá afectado de la limitación de la conexión.

#### <a name="evaluation-for-complex-use-cases"></a>Evaluación de los casos de uso complejo

##### <a name="bigger-message-size-or-different-sending-rate"></a>Velocidad de envío diferente o de mayor tamaño de mensaje

El caso de uso real es más complicado. Es posible que envíe un mensaje más de 2.048 bytes, o la tasa de mensajes de envío es no, un mensaje por segundo. Tomemos difusión del Unit100 como un ejemplo para descubrir cómo evaluar su rendimiento.

En la tabla siguiente se muestra un caso de uso real de **difusión**. Pero el tamaño del mensaje, recuento de conexiones y tasa de envío de mensajes son diferentes de lo que se supone que en la sección anterior. La cuestión es cómo podemos deducir alguno de esos elementos (tamaño del mensaje, recuento de conexiones o velocidad de envío de mensaje) Si conocemos sólo dos de ellos.

| Difusión  | Tamaño del mensaje | Mensajes entrantes por segundo | Conexiones | Intervalos de envío |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 KB                | 1                        | 100 000     | 5 segundos                      |
| 2 | 256 KB               | 1                        | 8.000       | 5 segundos                      |

La siguiente fórmula es fácil deducir basándose en la fórmula anterior:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Para Unit100, el máximo ancho de banda saliente es 400 MB de la tabla anterior. Para un tamaño de mensaje de 20 KB, el número máximo de conexiones de salida debe ser 400 MB \* 5 / 20 KB = 100 000, que coincide con el valor real.

##### <a name="mixed-use-cases"></a>Casos de uso mixto

El caso de uso real normalmente mezcla juntos los cuatro casos de uso básico: **echo**, **difusión**, **envío al grupo**, y **enviar a conexión**. Es la metodología que se utiliza para evaluar la capacidad de:

1. Dividir los casos de uso mixto en cuatro casos de uso básico.
1. Calcular el ancho de banda máximo del mensaje entrante y saliente mediante el uso de las fórmulas anteriores por separado.
1. Sumar los cálculos de ancho de banda para obtener el máximo de entrada/salida ancho de banda total. 

Elige el nivel adecuado de las tablas de entrada/salida de ancho de banda máximo.

> [!NOTE]
> Para enviar un mensaje a cientos o miles de grupos pequeños o para miles de clientes que envían un mensaje entre sí, el costo de enrutamiento estará dominante. Tomar este impacto en la cuenta.

Para el caso de uso de enviar un mensaje a los clientes, asegúrese de que el servidor de aplicaciones es *no* el cuello de botella. La siguiente sección de "Caso práctico" proporciona directrices acerca de cuántos servidores de la aplicación necesita y cuántas conexiones de servidor, debe configurar.

## <a name="case-study"></a>Caso práctico

Las siguientes secciones se vaya a través de los cuatro casos de uso típico de transporte de WebSocket: **echo**, **difusión**, **envío al grupo**, y **enviar a conexión**. Para cada escenario, la sección muestra la capacidad actual de entrada y salida para Azure SignalR Service. También explica los principales factores que afectan al rendimiento.

En el modo predeterminado, el servidor de la aplicación crea cinco conexiones de servidor con Azure SignalR Service. El servidor de la aplicación usa el SDK de Azure SignalR Service de forma predeterminada. En los siguientes resultados de pruebas de rendimiento, las conexiones del servidor se aumentan a 15 (o más para la difusión y enviar un mensaje a un grupo grande).

Casos de uso diferentes tienen requisitos diferentes para los servidores de aplicaciones. **Difusión** necesita un número pequeño de servidores de aplicaciones. **Echo** o **enviar a conexión** necesita varios servidores de aplicación.

En todos los casos de uso, el tamaño del mensaje predeterminado es 2048 bytes y el intervalo de envío del mensaje es de 1 segundo.

### <a name="default-mode"></a>Modo predeterminado

Azure SignalR Service, los servidores de aplicaciones web y los clientes están implicados en el modo predeterminado. Todos los clientes significa una sola conexión.

#### <a name="echo"></a>Echo

En primer lugar, una aplicación web se conecta a Azure SignalR Service. En segundo lugar, muchos clientes se conectan a la aplicación web, que redirige a los clientes a Azure SignalR Service con el token de acceso y el punto de conexión. A continuación, los clientes establecen conexiones de WebSocket con Azure SignalR Service.

Después de que todos los clientes establecen conexiones, comienzan a enviar un mensaje que contiene una marca de tiempo para el concentrador concreto cada segundo. El concentrador reproduce el mensaje de vuelta a su cliente original. Todos los clientes calcula la latencia cuando recibe el mensaje de eco de vuelta.

En el siguiente diagrama, 5 a 8 (tráfico resaltado rojo) se encuentran en un bucle. El bucle se ejecuta durante un tiempo predeterminado (5 minutos) y obtiene la estadística de todos los latencia de mensajes.

![Tráfico para el caso de uso de echo](./media/signalr-concept-performance/echo.png)

El comportamiento de **echo** determina que el máximo ancho de banda entrante es igual que el máximo ancho de banda saliente. Para obtener más información, consulte la tabla siguiente.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Mensajes entrantes y salientes por segundo | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Ancho de banda entrante y saliente | 2 MBps   | 4 MBps   | 10 MBps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |

En este caso de uso, cada cliente invoca el concentrador definido en el servidor de aplicaciones. El concentrador simplemente llama al método definido en el lado del cliente original. Este centro es el concentrador más ligero de **echo**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Incluso para este centro simple, la presión del tráfico en el servidor de aplicaciones es destacada como el **echo** aumenta la carga de mensaje de entrada. Esta presión de tráfico requiere muchos servidores de aplicaciones para grandes niveles SKU. La tabla siguiente muestra la cantidad de servidores de aplicación para cada nivel.


|    Echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El cliente Conexión número, tamaño del mensaje, mensaje de envío de CPU/memoria del servidor de aplicaciones, nivel de SKU y tasa de afectar al rendimiento global de **echo**.

#### <a name="broadcast"></a>Difusión

Para **difusión**, cuando la aplicación web recibe el mensaje, lo transmite a todos los clientes. Los clientes más allí son de difusión, el tráfico de mensajes más que hay a todos los clientes. Consulte el diagrama siguiente.

![Tráfico para el caso de uso de difusión](./media/signalr-concept-performance/broadcast.png)

Difunde un número pequeño de clientes. El ancho de banda de mensaje de entrada es pequeño, pero el ancho de banda saliente es enorme. El ancho de banda de mensaje saliente aumenta a medida que la conexión de cliente o aumenta la frecuencia de difusión.

En la tabla siguiente se resume las conexiones de cliente máximo, número de mensajes de entrada/salida y ancho de banda.

|     Difusión             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Mensajes salientes por segundo | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante  | 4 kBps   | 4 kBps   | 4 kBps    | 4 kBps    | 4 kBps    | 4 kBps     | 4 kBps     |
| Ancho de banda saliente | 4 MBps   | 8 MBps   | 20 MBps   | 40 MBps   | 80 MBps   | 200 MBps   | 400 MBps   |

Los clientes de difusión que registrar los mensajes son no más de cuatro. Necesitan menos servidores de aplicaciones en comparación con **echo** porque la cantidad de mensajes de entrada es pequeña. Dos servidores de aplicación son suficientes para conocer las consideraciones de SLA y el rendimiento. Pero se deben incrementar las conexiones del servidor de forma predeterminada para evitar el desequilibrio, especialmente para Unit50 y Unit100.

|   Difusión      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Aumentar las conexiones del servidor predeterminado de 5 a 40 en cada servidor de aplicaciones para evitar las conexiones de servidor no está equilibrada posible Azure SignalR Service.
>
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje y nivel de SKU afectan al rendimiento global para **difusión**.

#### <a name="send-to-group"></a>Envío al grupo

El **envío al grupo** caso de uso tiene un patrón de tráfico similar a **difusión**. La diferencia es que después de los clientes establecen conexiones de WebSocket con Azure SignalR Service, deben unirse a grupos antes de que puede enviar un mensaje a un grupo específico. El diagrama siguiente ilustra el flujo de tráfico.

![Tráfico para el caso de uso del grupo de envío](./media/signalr-concept-performance/sendtogroup.png)

Número de grupos y miembros del grupo son dos factores que afectan al rendimiento. Para simplificar el análisis, se definen dos tipos de grupos:

- **Grupo pequeño**: Cada grupo tiene 10 conexiones. El número de grupo es igual a (número máximo de conexiones) / 10. Por ejemplo, para Unit1, si hay 1.000 recuentos de conexión, a continuación, tenemos 1000 / 10 = 100 grupos.

- **Grupo grande**: El número de grupo siempre es 10. El número de miembros del grupo es igual a (número máximo de conexiones) / 10. Por ejemplo, para Unit1, si hay 1.000 recuentos de conexión, a continuación, cada grupo tiene 1000 / 10 = 100 miembros.

**Envío al grupo** aporta un enrutamiento de costos en Azure SignalR Service porque debe buscar las conexiones de destino a través de una estructura de datos distribuidos. A medida que aumentan las conexiones envío, aumenta el costo.

##### <a name="small-group"></a>Grupo pequeño

El costo de enrutamiento es significativo para enviar mensajes a muchos grupos pequeños. Actualmente, la implementación de Azure SignalR Service alcanza el límite de costo de enrutamiento en Unit50. Adición de más CPU y memoria no ayuda, por lo que no se puede mejorar Unit100 más, por diseño. Si tiene un ancho de banda entrante, póngase en contacto con soporte al cliente.

|   Enviar a un grupo pequeño     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000 | 100 000
| Número de miembros del grupo        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Número de grupos               | 100   | 200   | 500    | 1000  | 2.000  | 5.000  | 10 000 
| Mensajes entrantes por segundo  | 200   | 400   | 1000  | 2500  | 4.000  | 7000  | 7000   |
| Ancho de banda entrante  | 400 kBps  | 800 kBps  | 2 MBps     | 5 MBps     | 8 MBps     | 14 MBps    | 14 MBps     |
| Mensajes salientes por segundo | 2.000 | 4.000 | 10 000 | 25 000 | 40.000 | 70,000 | 70,000  |
| Ancho de banda saliente | 4 MBps    | 8 MBps    | 20 MBps    | 50 MBps     | 80 MBps    | 140 MBps   | 140 MBps    |

Número de conexiones de cliente está llamando el concentrador, por lo que el número de servidores de la aplicación también es fundamental para el rendimiento. En la tabla siguiente se enumera los recuentos de server app sugeridos.

|  Enviar a un grupo pequeño   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El cliente Conexión número, tamaño del mensaje, envía la velocidad, costo de enrutamiento, nivel de SKU y CPU/memoria del servidor de aplicación un mensaje afectan al rendimiento general de **enviar a un grupo pequeño**.

##### <a name="big-group"></a>Grupo grande

Para **enviar a un grupo grande**, el ancho de banda saliente se convierte en el cuello de botella antes de alcanzar el enrutamiento de límite de costo. La tabla siguiente muestra el ancho de banda máximo de salida, que es prácticamente el mismo que para **difusión**.

|    Enviar a un grupo grande      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000
| Número de miembros del grupo        | 100   | 200   | 500    | 1000  | 2.000  | 5.000   | 10 000 
| Número de grupos               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Mensajes entrantes por segundo  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Ancho de banda entrante  | 80 kBps   | 40 kBps   | 40 kBps    | 20 KB por segundo    | 40 kBps    | 40 kBps     | 40 kBps     |
| Mensajes salientes por segundo | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda saliente | 8 MBps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

El número de conexiones de envío es de no más de 40. La carga en el servidor de aplicaciones es pequeña, por lo que el número de aplicaciones web sugerido es pequeño.

|  Enviar a un grupo grande  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Aumentar las conexiones del servidor predeterminado de 5 a 40 en cada servidor de aplicaciones para evitar las conexiones de servidor no está equilibrada posible Azure SignalR Service.
> 
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje, costo de enrutamiento y nivel de SKU afectan al rendimiento general de **enviar a un grupo grande**.

#### <a name="send-to-connection"></a>Enviar a conexión

En el **enviar a conexión** caso de uso, cuando los clientes establecen las conexiones a Azure SignalR Service, cada cliente llama a un concentrador especial para obtener su propio identificador de conexión. El banco de pruebas de rendimiento recopila todos los identificadores de conexión, ordena a ellos y reasigna a todos los clientes como un destino de envío. Los clientes seguir enviando el mensaje a la conexión de destino hasta que finalice la prueba de rendimiento.

![Tráfico para el caso de uso de envío al cliente](./media/signalr-concept-performance/sendtoclient.png)

El enrutamiento de costo para **enviar a conexión** es similar al costo de **enviar a un grupo pequeño**.

A medida que aumenta el número de conexiones, el costo de enrutamiento limita el rendimiento general. Unit50 ha alcanzado el límite. Como resultado, no se puede mejorar aún más el Unit100.

La tabla siguiente es un resumen estadístico tras varias etapas de la ejecución del **enviar a conexión** banco de pruebas.

|   Enviar a conexión   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Conexiones                        | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000          | 100 000         |
| Mensajes entrantes y salientes por segundo | 1000 | 2.000 | 5.000 | 8.000  | 9000  | 20.000 | 20.000 |
| Ancho de banda entrante y saliente | 2 MBps    | 4 MBps    | 10 MBps   | 16 MBps    | 18 MBps    | 40 MBps       | 40 MBps       |

En este caso de uso requiere una carga elevada en el lado del servidor de aplicación. Ver el servidor de aplicaciones sugeridas count en la tabla siguiente.

|  Enviar a conexión  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El cliente Conexión número, tamaño del mensaje, el mensaje de envío de velocidad, costo de enrutamiento, nivel de SKU y CPU/memoria para el servidor de aplicaciones afectar al rendimiento global de **enviar a conexión**.

#### <a name="aspnet-signalr-echo-broadcast-and-send-to-small-group"></a>ASP.NET SignalR eco, difusión y envío a un grupo pequeño

Azure SignalR Service proporciona la misma capacidad de rendimiento de SignalR de ASP.NET. 

La prueba de rendimiento utiliza Azure Web Apps desde [S3 de Plan de servicio estándar](https://azure.microsoft.com/pricing/details/app-service/windows/) para ASP.NET SignalR.

En la tabla siguiente ofrece el recuento de la aplicación web sugerido para ASP.NET Signalren **echo**.

|   Echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

En la tabla siguiente ofrece el recuento de la aplicación web sugerido para ASP.NET Signalren **difusión**.

|  Difusión       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

En la tabla siguiente ofrece el recuento de la aplicación web sugerido para ASP.NET Signalren **enviar a un grupo pequeño**.

|  Enviar a un grupo pequeño     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

### <a name="serverless-mode"></a>Modo sin servidor

Los clientes y Azure SignalR Service están implicados en el modo sin servidor. Todos los clientes significa una sola conexión. El cliente envía mensajes a través de la API de REST a otro cliente o difusión de mensajes a todos.

Envío de mensajes de alta densidad a través de la API de REST no es tan eficaz como el uso de WebSocket. Requiere crear una nueva conexión HTTP cada vez y que es un costo adicional en modo sin servidor.

#### <a name="broadcast-through-rest-api"></a>Difundir a través de la API de REST
Todos los clientes establecen conexiones de WebSocket con Azure SignalR Service. A continuación, algunos clientes de inician la difusión a través de la API de REST. Es el mensaje de envío (entrante), todo mediante HTTP Post, que no es eficaz en comparación con WebSocket.

|   Difundir a través de la API de REST     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Mensajes salientes por segundo | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante  | 4 kBps    | 4 kBps    | 4 kBps     | 4 kBps     | 4 kBps     | 4 kBps      | 4 kBps      |
| Ancho de banda saliente | 4 MBps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

#### <a name="send-to-user-through-rest-api"></a>Enviar al usuario a través de la API de REST
El criterio de referencia asigna los nombres de usuario para todos los clientes antes de empezar a conectarse a Azure SignalR Service. Después de que los clientes establecen conexiones de WebSocket con Azure SignalR Service, comienzan a enviar mensajes a otros usuarios a través de HTTP Post.

|   Enviar al usuario a través de la API de REST | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 300   | 600   | 900    | 1,300  | 2.000  | 10 000  | 18,000  |
| Mensajes salientes por segundo | 300   | 600   | 900    | 1,300  | 2.000  | 10 000  | 18,000 |
| Ancho de banda entrante  | 600 kBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 MBps     | 10 MBps     | 36 MBps    |
| Ancho de banda saliente | 600 kBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 MBps     | 10 MBps     | 36 MBps    |

## <a name="performance-test-environments"></a>Entornos de prueba de rendimiento

Para todos los casos mencionados anteriormente de uso, hemos llevado a cabo las pruebas de rendimiento en un entorno de Azure. A lo sumo, hemos usado 50 máquinas virtuales del cliente y 20 máquinas virtuales del servidor de aplicaciones. Estos son algunos detalles:

- Tamaño de máquina virtual de cliente: StandardDS2V2 (2 vCPU, memoria 7G)

- Tamaño de máquina virtual del servidor de aplicaciones: StandardF4sV2 (4 vCPU, memoria de 8G)

- Conexiones del servidor de Azure SDK de SignalR: 15

## <a name="performance-tools"></a>Herramientas de rendimiento

Puede encontrar herramientas de rendimiento para Azure SignalR Service en [GitHub](https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se obtuvo una visión general de rendimiento de Azure SignalR Service en los escenarios de casos de uso típicos.

Para obtener detalles sobre el funcionamiento interno del servicio y el escalado, lea las siguientes guías:

* [Aspectos internos de Azure SignalR Service](signalr-concept-internals.md)
* [Escalado de Azure SignalR Service](signalr-howto-scale-multi-instances.md)
