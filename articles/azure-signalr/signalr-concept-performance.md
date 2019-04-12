---
title: Guía de rendimiento para Azure SignalR Service
description: Información general de rendimiento del servicio de Azure SignalR.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: 53139dd253c491ea6578fd0b9cbada4e7b331c7d
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502044"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Guía de rendimiento para Azure SignalR Service

Una de las ventajas principales para usar Azure SignalR Service es la facilidad para escalar aplicaciones SignalR. En un escenario a gran escala, rendimiento, se convierte en un factor importante. En esta guía se explicarán los factores que tengan impacto en el rendimiento de la aplicación de SignalR y, en escenarios de casos de uso diferentes, ¿qué es el rendimiento normal? Al final, también presentaremos el entorno y herramientas que se usan para generar el informe de rendimiento.

## <a name="terms-definition"></a>Definición de términos

*ASRS*: Servicio Azure SignalR

*Entrada*: el mensaje entrante a Azure SignalR Service

*Salida*: el mensaje saliente de Azure SignalR Service

*Ancho de banda*: tamaño total de todos los mensajes en 1 segundo

*Modo predeterminado*: ASRS espera que el servidor de aplicaciones para establecer conexión con él antes de aceptar las conexiones de cliente. Es el modo de funcionamiento predeterminado cuando se creó un ASRS.

*Modo sin servidor*: ASRS solo acepta conexiones de cliente. No se permite ninguna conexión de servidor.

## <a name="overview"></a>Información general

ASRS define siete niveles estándar para las capacidades de rendimiento diferente y tiene la intención de esta guía responder a las preguntas siguientes:

-   ¿Qué es el rendimiento ASRS típico para cada nivel?

-   ¿ASRS cumple el requisito de rendimiento de mensajes, por ejemplo, envío de 100 000 mensajes por segundo?

-   ¿Para mi escenario concreto, qué nivel es adecuado para mí? O bien, ¿cómo puedo seleccionar el nivel adecuado?

-   ¿Qué tipo de servidor de aplicaciones (tamaño de máquina virtual) es adecuada para mí y cuántos de ellos serán implementar?

Para responder a estas preguntas, esta guía de rendimiento primero proporciona una explicación detallada acerca de los factores que afectan el rendimiento, a continuación, se muestra el número máximo de mensajes entrante y saliente para cada nivel para casos de uso típicos: **eco**, **difusión**, **envío al grupo**, y **enviar a conexión** (charla de punto a punto).

Es imposible de este documento cubrir todos los escenarios (y caso de uso diferentes, el tamaño de mensaje diferente o patrón de envío de mensajes etcetera.). Sin embargo, proporciona algunos métodos de evaluación para ayudar a los usuarios a aproximadamente evaluar sus requisitos de los mensajes entrantes o salientes y, después, busque los niveles adecuados mediante la comprobación de la tabla de rendimiento.

## <a name="performance-insight"></a>Información de rendimiento

En esta sección se describe las metodologías de evaluación del rendimiento, a continuación, se enumera todos los factores que afectan el rendimiento. Al final, proporciona métodos para ayudar a evaluar los requisitos de rendimiento.

### <a name="methodology"></a>Metodología

**Rendimiento** y **latencia** típico consta de la comprobación de rendimiento. Para ASRS, nivel de SKU diferentes tiene distintos niveles de rendimiento directiva de limitación. Este documento se define **el máximo permitido de rendimiento (ancho de banda entrante y saliente)** como el número máximo de rendimiento obtenido al 99% de los mensajes de tener una latencia inferior a 1 segundo.

La latencia es el intervalo de tiempo de la conexión que envía el mensaje para recibir el mensaje de respuesta de ASRS. Vamos a echar **echo** como por ejemplo, cada conexión de cliente agrega una marca de tiempo en el mensaje. Concentrador del servidor de la aplicación envía el mensaje original al cliente. Por lo tanto, el retraso de propagación fácilmente se calcula por cada conexión de cliente. La marca de tiempo se adjunta para todos los mensajes de **difusión**, **envío al grupo**, y **enviar a conexión**.

Para simular miles de conexiones de clientes simultáneos, varias máquinas virtuales se crean en una red privada virtual en Azure. Todas estas máquinas virtuales se conectarán a la misma instancia ASRS.

En el modo predeterminado ASRS, también se implementan máquinas virtuales del servidor de aplicaciones en la misma red privada virtual como máquinas virtuales del cliente.

Todas las máquinas virtuales del cliente y del servidor de aplicaciones que se implementan máquinas virtuales en la misma red de la misma región para evitar que entre la latencia de región.

### <a name="performance-factors"></a>Factores de rendimiento

En teoría, capacidad ASRS está limitado por los recursos de cálculo: CPU, memoria y red. Por ejemplo, más conexiones a ASRS, consume más memoria ASRS. Para mayor tráfico de mensajes, por ejemplo, todos los mensajes es mayor que 2048 bytes, requiere ASRS gastar más ciclos de CPU para procesar también. Mientras tanto, el ancho de banda de red de Azure también impone un límite para el tráfico máximo.

El tipo de transporte [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [evento enviado del servidor](https://en.wikipedia.org/wiki/Server-sent_events), o [sondeo largo](https://en.wikipedia.org/wiki/Push_technology), es otro factor afecta al rendimiento. WebSocket es un protocolo de comunicación bidireccional y dúplex completo en una única conexión TCP. Sin embargo, evento enviado del servidor es unidireccional protocolo mensaje de inserción del servidor al cliente. Sondeo largo requiere que los clientes que éstos sondeen periódicamente la información del servidor a través de la solicitud HTTP. Para la misma API en la misma condición, WebSocket tiene el mejor rendimiento, eventos enviados del servidor es más lento y sondeo largo es el más lento. ASRS recomienda WebSocket de forma predeterminada.

Además, el costo de enrutamiento de mensajes también limita el rendimiento. ASRS desempeña un papel como un enrutador de mensajes, que enruta el mensaje de un conjunto de clientes o servidores a otros clientes o servidores. Escenario diferente o la API requiere la directiva de enrutamiento distintos. Para **echo**, el cliente envía un mensaje a sí mismo y el destino del enrutamiento es en sí mismo. Este patrón tiene el menor costo de enrutamiento. Pero para **difusión**, **envío al grupo**, **conexión de envío**, ASRS debe buscar las conexiones de destino a través de la estructura interna de datos distribuidos, que consume más CPU, memoria y ancho de banda de red incluso. Como resultado, el rendimiento es más lento que **echo**.

En el modo predeterminado, el servidor de aplicaciones también puede quedar un cuello de botella para determinados escenarios, ya tiene SDK de Azure SignalR invocar el concentrador, mientras tanto mantiene la conexión en vivo con todos los clientes a través de las señales de latido.

En el modo sin servidor, el cliente envía mensajes mediante HTTP post, que no es tan eficaz como WebSocket.

Otro factor es el protocolo: JSON y [MessagePack](https://msgpack.org/index.html). MessagePack es un tamaño reducido y entregado con más rapidez que JSON. De manera intuitiva, MessagePack beneficiaría rendimiento, pero rendimiento ASRS no es información confidencial con protocolos puesto que no descodifique la carga del mensaje durante el reenvío de mensajes de los clientes a servidores o viceversa.

En resumen, los siguientes factores tengan impacto en la capacidad de entrada y salida:

-   Nivel de SKU (CPU y memoria)

-   Número de conexiones

-   Tamaño del mensaje

-   tasa de envío de mensajes

-   transport type (WebSocket/Sever-Sent-Event/Long-Polling)

-   Utilice el escenario de caso (costo de enrutamiento)

-   conexiones de servidor y el servicio de aplicación (en modo de servidor)


### <a name="find-a-proper-sku"></a>Encontrar una SKU adecuada

¿Cómo evaluar la capacidad de entrada/salida o cómo buscar qué nivel es adecuado para un caso de uso específico?

Se supone que el servidor de aplicaciones es lo suficientemente eficaz y no es el cuello de botella de rendimiento. A continuación, podemos comprobar el máximo ancho de banda entrante y saliente para cada nivel.

#### <a name="quick-evaluation"></a>Evaluación rápida

Vamos a simplificar la evaluación en primer lugar, suponiendo algunos valores predeterminados: WebSocket se utiliza, tamaño del mensaje es de 2048 bytes, enviar mensaje de 1 segundo, y es de forma predeterminada en modo.

Cada nivel tiene su propio máximo ancho de banda entrante y saliente. No se garantiza la experiencia de usuario cuando la entrada o salida supera el límite.

**Echo** ofrece el máximo ancho de banda entrante porque tiene el menor costo de enrutamiento. **Difusión** define el ancho de banda máximo del mensaje saliente.

Hacer **no** superan los valores resaltados en las dos tablas siguientes.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| **Ancho de banda entrante (bytes/seg.)** | **2M**    | **4M**    | **10 MILLONES**   | **20M**    | **40M**    | **100M**   | **200M**    |
| Ancho de banda de salida (bytes/seg.) | 2M    | 4M    | 10 MILLONES   | 20M    | 40M    | 100M   | 200M    |


|     Difusión             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Ancho de banda entrante (bytes/seg.)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K     |
| **Ancho de banda de salida (bytes/seg.)** | **4M**    | **8M**    | **20M**    | **40M**    | **80M**    | **200M**    | **400M**   |

El ancho de banda entrante y las fórmulas de ancho de banda saliente:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

*inboundConnections*: el número de conexiones y enviará el mensaje

*outboundConnections*: el número de conexiones de recepción de mensaje

*messageSize*: el tamaño de un único mensaje (valor medio). Para mensaje pequeño cuyo tamaño es inferior a 1024 bytes, tiene el impacto de rendimiento similares como mensaje de 1024 bytes.

*sendInterval*: la hora de enviar un mensaje, normalmente es 1 segundo por cada mensaje, lo que implica enviar un mensaje cada segundo. SendInterval más pequeño significa enviar más mensajes determinado período de tiempo. Por ejemplo, 0,5 segundos por cada mensaje significa enviar dos mensajes cada segundo.

*Las conexiones* es el umbral máximo confirmado de ASRS para cada nivel. Si el número de conexiones aumenta aún más, se verá afectado de la limitación de la conexión.

*Ancho de banda de entrada* y *ancho de banda saliente* tienen el tamaño total de mensajes por segundo. Aquí estoy ' significa megabytes por motivos de simplicidad.

#### <a name="evaluation-for-complex-use-cases"></a>Evaluación de los casos de uso complejo

##### <a name="bigger-message-size-or-different-sending-rate"></a>Velocidad de envío diferente o de mayor tamaño de mensaje

El caso de uso real es más complicado. Puede enviar un mensaje mayor que 2048 bytes o tasa de envío de mensajes es no, un mensaje por segundo. Tomemos difusión del unit100 como un ejemplo para descubrir cómo evaluar su rendimiento.

En la tabla siguiente se muestra un caso real de **difusión**, pero el tamaño del mensaje, recuento de conexiones y tasa de envío de mensajes son diferentes de lo que se supone que en la sección anterior. La cuestión es cómo podemos deducir alguno de esos elementos (tamaño del mensaje, recuento de conexiones o velocidad de envío de mensaje) Si conocemos sólo 2 de ellos.

| Difusión  | Tamaño del mensaje (bytes) | Entrada (mensajes/s) | Conexiones | Enviar intervalos (segundo) |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 K                 | 1                        | 100 000     | 5                       |
| 2 | 256 K                | 1                        | 8.000       | 5                       |

La siguiente fórmula es fácilmente para que se puede inferir basándose en la fórmula anterior existente:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Para unit100, sabemos que el máximo ancho de banda saliente es 400M de la tabla anterior, para el tamaño del mensaje de 20 K, el número máximo de conexiones de salida debe ser 400M \* 5 / 20 K = 100.000, que coincide con el valor real.

##### <a name="mixed-use-cases"></a>Casos de uso mixto

El caso de uso real normalmente mezcla juntos los cuatro casos de uso básico: **echo**, **difusión**, **envío al grupo**, o **enviar a conexión**. La metodología empleada para evaluar la capacidad consiste en dividir los casos de uso mixto en cuatro casos de uso básico, **calcular el ancho de banda máximo del mensaje entrante y saliente** utilizando las fórmulas anteriores por separado y los suma para obtener el total máximo ancho de banda entrante y saliente. Elige el nivel adecuado de las tablas de entrada/salida de ancho de banda máximo.

Mientras tanto, para enviar mensajes a cientos o miles de grupos pequeños o miles de clientes que envían mensajes entre sí, el costo de enrutamiento se convertirá en dominante. Este impacto debería tenerse en cuenta. Obtener más detalles se tratan en las secciones siguientes de "Caso práctico".

Para el caso de uso de enviar mensajes a los clientes, asegúrese de que el servidor de aplicaciones es **no** el cuello de botella. Sección "Caso práctico" proporciona las directrices sobre cuántos servidores de la aplicación necesita y se debe configurar el número de conexiones de servidor.

## <a name="case-study"></a>Caso práctico

Las siguientes secciones se vaya a través de los cuatro casos de uso típico de transporte de WebSocket: **echo**, **difusión**, **envío al grupo**, y **conexión de envío** . Para cada escenario, muestra el ASRS actual de entrada y la capacidad de salida, mientras tanto, explica qué es los principales factores en el rendimiento.

De forma predeterminada en modo de servidor de aplicaciones, a través del SDK de Azure SignalR Service de forma predeterminada, crea cinco conexiones de servidor con ASRS. En el rendimiento de resultado de la prueba siguiente, se incrementan las conexiones a 15 (o más para la difusión y enviar el mensaje a un grupo grande) del servidor.

Distintos casos de uso tienen requisitos diferentes en los servidores de aplicaciones. **Difusión** necesita un número pequeño de servidores de aplicaciones. **Echo** o **conexión de envío** necesita varios servidores de aplicación.

En todos los casos de uso, el tamaño predeterminado del mensaje es de 2048 bytes y el intervalo de envío de mensajes es de 1 segundo.

## <a name="default-mode"></a>Modo predeterminado

Los clientes, servidores de aplicaciones web y ASRS están implicados en este modo. Todos los clientes significa una sola conexión.

### <a name="echo"></a>Echo

En primer lugar, las aplicaciones web se conectan a ASRS. En segundo lugar, muchos clientes se conectan a la aplicación web, que redirigir a los clientes a ASRS con el token de acceso y el punto de conexión. A continuación, los clientes establezcan conexiones WebSocket con ASRS.

Después de que todos los clientes establecen conexiones, comienzan a enviar el mensaje, que contiene una marca de tiempo para el concentrador concreto cada segundo. El concentrador reproduce el mensaje de vuelta a su cliente original. Todos los clientes calcula la latencia cuando recibe el mensaje de eco de vuelta.

Los pasos 5\~8 (tráfico resaltado rojo) se encuentran en un bucle, lo que se ejecutará durante un tiempo predeterminado (5 minutos) y obtener la estadística de todos los latencia de mensajes.
La Guía de rendimiento muestra el máximo número de conexiones de cliente.

![Echo](./media/signalr-concept-performance/echo.png)

**Echo**del comportamiento determina que el máximo ancho de banda entrante es igual al ancho de banda de salida máximo. Consulte la tabla siguiente.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Entrante o saliente (mensajes/s) | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Ancho de banda de entrada y salida (bytes/seg.) | 2M    | 4M    | 10 MILLONES   | 20M    | 40M    | 100M   | 200M    |

En este caso de uso, cada cliente invoca el concentrador definido en el servidor de aplicaciones. El concentrador simplemente llama al método definido en el lado del cliente original. Este centro es el concentrador pesado más claro de **echo**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Incluso para este centro simple, la presión del tráfico en el servidor de aplicaciones también es destacada como el **echo** aumenta de mensaje de entrada. Por lo tanto, requiere muchos servidores de aplicaciones para grandes niveles SKU. La tabla siguiente muestra la cantidad de servidores de aplicación para cada nivel.


|    Echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> El cliente Conexión número, tamaño del mensaje, mensaje de envío de velocidad, nivel de SKU y de CPU/memoria del servidor de aplicaciones tener impacto en el rendimiento general de **echo**.

### <a name="broadcast"></a>Difusión

Para **difusión**, cuando la aplicación web recibe el mensaje, lo transmite a todos los clientes. Los clientes más para difundir el tráfico de mensajes más a todos los clientes. Consulte el diagrama siguiente.

![Difusión](./media/signalr-concept-performance/broadcast.png)

La característica de difusión es que hay un número pequeño de clientes de difusión, lo que significa que el ancho de banda de mensaje de entrada es pequeño, pero el ancho de banda saliente es enorme. El ancho de banda de mensaje saliente aumenta a medida que la conexión de cliente o aumenta la frecuencia de difusión.

En la tabla siguiente se resumen las conexiones de cliente máximas, número de mensaje entrante y saliente y ancho de banda.

|     Difusión             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Entrada (mensajes/s)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Salida (mensajes/s) | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante (bytes/seg.)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Ancho de banda de salida (bytes/seg.) | 4M    | 8M    | 20M    | 40M    | 80M    | 200M    | 400M    |

Los clientes de difusión que publican mensajes no más de 4, por lo tanto requiere menos servidores de aplicaciones en comparación con **echo** puesto que la cantidad de mensajes de entrada es pequeña. Dos servidores de aplicación son suficientes para la consideración del SLA y el rendimiento. Pero deben aumentarse las conexiones del servidor de forma predeterminada para evitar el problema no está equilibrado especialmente para Unit50 y Unit100.

|   Difusión      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> Aumentar las conexiones del servidor predeterminado de 5 a 40 en cada servidor de aplicaciones para evitar las conexiones de servidor no está equilibrada posible ASRS.
>
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje y nivel de SKU tienen impacto en el rendimiento general de **difusión**

### <a name="send-to-group"></a>Envío al grupo

**Grupo de envío** tiene el patrón de tráfico similar, salvo que después de establecer conexiones de WebSocket con ASRS los clientes, debe unirse a grupos antes de que puede enviar el mensaje a un grupo específico. En el diagrama siguiente, se ilustra el flujo de tráfico.

![Enviar al grupo](./media/signalr-concept-performance/sendtogroup.png)

Número de grupos y miembros del grupo son dos factores con un impacto en el rendimiento. Para simplificar el análisis, definimos dos tipos de grupos: grupos pequeños y grandes.

- `small group`: 10 conexiones en cada grupo. El número de grupo es igual a (número máximo de conexiones) / 10. Por ejemplo, para 1 unidad, si no hay recuentos de 1000 conexiones, a continuación, tenemos 1000 / 10 = 100 grupos.

- `Big group`: Número de grupo siempre es 10. El número de miembros del grupo es igual a (número máximo de conexiones) / 10. Por ejemplo, para 1 unidad, si no hay recuentos de 1000 conexiones, a continuación, cada grupo tiene 1000 / 10 = 100 miembros.

**Grupo de envío** aporta el costo de enrutamiento a ASRS porque debe buscar las conexiones de destino a través de una estructura de datos distribuidos. A medida que aumentan las conexiones envío, también aumenta el costo.

#### <a name="small-group"></a>Grupo pequeño

El costo de enrutamiento es significativo para enviar mensajes a muchos grupos pequeños. Actualmente, la implementación de ASRS alcanza el límite de costo de enrutamiento en unit50. Adición de más CPU y memoria no ayuda, por lo que no se puede mejorar unit100 más, por diseño. Si la demanda de ancho de banda entrante más, póngase en contacto con soporte al cliente para la personalización.

|   Enviar a un grupo pequeño     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000 | 100 000
| Número de miembros del grupo        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Número de grupos               | 100   | 200   | 500    | 1000  | 2.000  | 5.000  | 10 000 
| Entrada (mensajes/s)  | 200   | 400   | 1000  | 2500  | 4.000  | 7000  | 7000   |
| Ancho de banda entrante (bytes/seg.)  | 400 K  | 800 K  | 2M     | 5M     | 8M     | 14M    | 14M     |
| Salida (mensajes/s) | 2.000 | 4.000 | 10 000 | 25 000 | 40.000 | 70,000 | 70,000  |
| Ancho de banda de salida (bytes/seg.) | 4M    | 8M    | 20M    | 50 MILLONES     | 80M    | 140M   | 140M    |

Hay muchas conexiones de cliente, una llamada del concentrador, por lo tanto, el número del servidor de aplicación también es fundamental para el rendimiento. El número de servidores de aplicación sugeridos se muestra en la tabla siguiente.

|  Enviar a un grupo pequeño   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje, costo de enrutamiento, nivel de SKU y de CPU/memoria del servidor de aplicaciones tienen impacto en el rendimiento general de **envío de grupos pequeños**.

#### <a name="big-group"></a>Grupo grande

Para **send-a-big-group**, el ancho de banda saliente se convierte en el cuello de botella antes de alcanzar el enrutamiento de límite de costo. La tabla siguiente muestra el ancho de banda máximo de salida, que es prácticamente el mismo como **difusión**.

|    Enviar a un grupo grande      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000
| Número de miembros del grupo        | 100   | 200   | 500    | 1000  | 2.000  | 5.000   | 10 000 
| Número de grupos               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Entrada (mensajes/s)  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Ancho de banda entrante (bytes/seg.)  | 80 K   | 40 K   | 40 K    | 20 K    | 40 K    | 40 K     | 40 K     |
| Salida (mensajes/s) | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda de salida (bytes/seg.) | 8M    | 8M    | 20M    | 40M    | 80M    | 200M    | 400M    |

El número de conexiones de envío es no más de 40, la carga del servidor de aplicaciones es pequeña, por lo tanto el número de la aplicación web sugerido también es pequeño.

|  Enviar a un grupo grande  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> Aumentar las conexiones del servidor predeterminado de 5 a 40 en cada servidor de aplicaciones para evitar las conexiones de servidor no está equilibrada posible ASRS.
> 
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje, costo de enrutamiento y nivel de SKU tienen impacto en el rendimiento general de **send-a-big-group**.

### <a name="send-to-connection"></a>Enviar a conexión

En este caso de uso, cuando los clientes establecen las conexiones a ASRS, cada cliente llama a un concentrador especial para obtener su propio identificador de conexión. El banco de pruebas de rendimiento es responsable de recopilar todos los identificadores de conexión, ordenar aleatoriamente ellos y vuelva a asignarlos a todos los clientes como un destino de envío. Los clientes seguir enviando mensajes a la conexión de destino hasta que finalice la prueba de rendimiento.

![Enviar al cliente](./media/signalr-concept-performance/sendtoclient.png)

El enrutamiento de costo para **conexión de envío** es similar a como **envío de grupos pequeños**.

A medida que aumenta el número de conexiones, el rendimiento general está limitado por costo de enrutamiento. Unidad de 50 ha alcanzado el límite. Como resultado, unidad de 100 no se puede mejorar aún más.

La tabla siguiente es un resumen estadístico después de muchas rondas de ejecución **conexión de envío** banco de pruebas

|   Enviar a conexión   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Conexiones                        | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000          | 100 000         |
| Entrante y saliente (mensajes/s) | 1000 | 2.000 | 5.000 | 8.000  | 9000  | 20.000 | 20.000 |
| Ancho de banda de entrada / salida (bytes/seg.) | 2M    | 4M    | 10 MILLONES   | 16M    | 18M    | 40M       | 40M       |

En este caso de uso requiere una carga elevada en el lado del servidor de aplicación. Ver el servidor de aplicaciones sugeridas count en la tabla siguiente.

|  Enviar a conexión  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> El número de conexiones de cliente, el tamaño de mensaje, velocidad de envío de mensaje, costo de enrutamiento, nivel de SKU y de CPU/memoria del servidor de aplicaciones tienen impacto en el rendimiento general de **enviar a conexión**.

### <a name="aspnet-signalr-echobroadcastsend-to-connection"></a>ASP.NET SignalR eco/difusión/envío a conexión

ASRS proporciona la misma capacidad de rendimiento de SignalR de ASP.NET. Esta sección ofrece el recuento de la aplicación web sugerido para ASP.NET Signalren **echo**, **difusión**, y **envío de grupos pequeños**.

La prueba de rendimiento utiliza Azure Web App de [S3 de Plan de servicio estándar](https://azure.microsoft.com/pricing/details/app-service/windows/) para ASP.NET SignalR.

- `echo`

|   Echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

- `broadcast`

|  Difusión       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

- `Send-to-small-group`

|  Enviar a un grupo pequeño     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1000 | 2.000 | 5.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicación | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

## <a name="serverless-mode"></a>Modo sin servidor

Los clientes y ASRS están implicados en este modo. Todos los clientes significa una sola conexión. El cliente envía mensajes a través de la API de REST a otro cliente o difusión de mensajes a todos.

Envío de mensajes de alta densidad a través de la API de REST no es tan eficaz como WebSocket, porque es necesario para crear una nueva conexión HTTP cada hora: costo en modo sin servidor adicional.

### <a name="broadcast-through-rest-api"></a>Difundir a través de la API de REST
Todos los clientes establecen conexiones de WebSocket con ASRS. A continuación, algunos clientes de inician la difusión a través de la API de REST. El mensaje de envío (entrante) son todo mediante HTTP Post, que no es eficaz en comparación con WebSocket.

|   Difundir a través de la API de REST     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Entrada (mensajes/s)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Salida (mensajes/s) | 2.000 | 4.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante (bytes/seg.)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Ancho de banda de salida (bytes/seg.) | 4M    | 8M    | 20M    | 40M    | 80M    | 200M    | 400M    |

### <a name="send-to-user-through-rest-api"></a>Enviar al usuario a través de la API de REST
La prueba comparativa asigna los nombres de usuario para todos los clientes antes de empezar a conectarse a ASRS. Después de los clientes establecieron conexiones de WebSocket con ASRS, comienzan a enviar mensajes a otros usuarios a través de HTTP Post.

|   Enviar al usuario a través de la API de REST | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1000 | 2.000 | 5.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Entrada (mensajes/s)  | 300   | 600   | 900    | 1,300  | 2.000  | 10 000  | 18,000  |
| Salida (mensajes/s) | 300   | 600   | 900    | 1,300  | 2.000  | 10 000  | 18,000 |
| Ancho de banda entrante (bytes/seg.)  | 600 K  | 1.2 M  | 1.8 M   | 2.6 M   | 4M     | 10 MILLONES     | 36M    |
| Ancho de banda de salida (bytes/seg.) | 600 K  | 1.2 M  | 1.8 M   | 2.6 M   | 4M     | 10 MILLONES     | 36M    |

## <a name="performance-test-environments"></a>Entornos de prueba de rendimiento

La prueba de rendimiento para todos los casos de uso mencionados anteriormente se realizaron en el entorno de Azure. En la mayoría de máquinas virtuales de cliente de 50 y 20 del servidor de aplicaciones se utilizan las máquinas virtuales.

Tamaño de máquina virtual de cliente: StandardDS2V2 (2 vCPU, memoria 7G)

Tamaño de máquina virtual del servidor de aplicaciones: StandardF4sV2 (4 vCPU, memoria de 8G)

Conexiones del servidor de Azure SDK de SignalR: 15

## <a name="performance-tools"></a>Herramientas de rendimiento

https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin

## <a name="next-steps"></a>Pasos siguientes

En este artículo, obtendrá una visión general de rendimiento de SignalR Service en escenarios de casos de uso típicos.

Para obtener más información acerca de los aspectos internos de SignalR Service y el escalado de SignalR Service, lea a la guía siguiente.

* [Funcionamiento interno del servicio Azure SignalR](signalr-concept-internals.md)
* [Escalado de Azure SignalR Service](signalr-howto-scale-multi-instances.md)