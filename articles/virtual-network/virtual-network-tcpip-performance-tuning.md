---
title: Optimización del rendimiento de TCP/IP para máquinas virtuales de Azure | Microsoft Docs
description: Obtenga información sobre diversas TCP/IP performance tuning técnicas comunes y su relación con máquinas virtuales de Azure.
services: virtual-network
documentationcenter: na
author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
manager: paragk
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/30/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 664c8b659152a370d7fb31907b6cdbcd414dce31
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905110"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP optimizar el rendimiento de las máquinas virtuales de Azure

El propósito de este artículo es explicar las técnicas de optimización de rendimiento de TCP/IP comunes y sus consideraciones para las máquinas virtuales que se ejecutan en Microsoft Azure. Es importante en primer lugar tener un conocimiento básico de los conceptos y a continuación, analizaremos cómo pueden optimizarse.

## <a name="common-tcpip-tuning-techniques"></a>Técnicas de optimización de TCP/IP comunes

### <a name="mtu-fragmentation-and-large-send-offload-lso"></a>MTU, la fragmentación y la descarga de envío grande (LSO)

#### <a name="explanation-of-mtu"></a>Explicación de MTU

La unidad de transmisión máxima (MTU) es el marco de tamaño más grande (paquete), especificado en bytes, que pueden enviarse a través de una interfaz de red. La MTU es un valor configurable y el valor predeterminado usada MTU en máquinas virtuales de Azure y la configuración predeterminada en la mayoría de los dispositivos de red es globalmente, 1500 bytes.

#### <a name="explanation-of-fragmentation"></a>Explicación de fragmentación

La fragmentación se produce cuando se envía un paquete que supera la MTU de una interfaz de red. La pila TCP/IP interrumpirá el paquete en partes más pequeñas (fragmentos) que se ajustan a las interfaces de MTU. La fragmentación se produce en el nivel de IP y es independiente del protocolo subyacente (por ejemplo, TCP). Cuando un paquete de 2000 bytes se envían a través de una interfaz de red con una MTU de 1500, a continuación se divide en un paquete de 1500 bytes y un paquete de 500 bytes.

Dispositivos de red en la ruta de acceso entre un origen y destino tienen la opción para quitar los paquetes que superen la MTU o fragmentar el paquete en partes más pequeñas.

#### <a name="the-dont-fragment-df-bit-in-an-ip-packet"></a>El bit "no fragmentar (DF)" en un paquete IP

El bit no fragmentar es una marca en el encabezado de protocolo IP. Cuando se establece el bit de DF, indica que los dispositivos de red intermedios en la ruta de acceso entre el remitente y receptor deben no fragmentar el paquete. Hay muchas razones por qué se puede establecer este bit (consulte la sección de detección de la ruta de acceso a continuación para obtener un ejemplo). Cuando un dispositivo de red recibe un paquete con el bit no fragmentar establecido y ese paquete supera la MTU de la interfaz de dispositivos, el comportamiento estándar es para que el dispositivo quitar el paquete y enviar un paquete de "Fragmentación ICMP necesaria" en el origen de la paquete.

#### <a name="performance-implications-of-fragmentation"></a>Implicaciones de rendimiento de fragmentación

La fragmentación puede afectar al rendimiento negativo. Una de las razones principales para el impacto en el rendimiento es el impacto de CPU/memoria de la fragmentación y reensamblado de paquetes. Cuando un dispositivo de red necesita un paquete de fragmentos, tendrá que asignar recursos de CPU/memoria para llevar a cabo la fragmentación. Lo mismo debe suceder cuando se vuelve a ensamblar el paquete. El dispositivo de red debe almacenar todos los fragmentos hasta que se reciben para reensamblarlas en el paquete original. Este proceso de fragmentación y reensamblado también puede producir latencia debido al proceso de fragmentación y reensamblado.

La otra implicación de posibles negativo en el rendimiento de fragmentación es que los paquetes fragmentados pueden llegar desordenados. Paquetes fuera de servicio pueden provocar que ciertos tipos de dispositivos de red para quitar los paquetes fuera de servicio - lo que requieren, a continuación, retransmitir todo el paquete. Escenarios típicos para quitar fragmentos son los dispositivos de seguridad, como los firewalls de red o cuando un dispositivo de red de la recepción se agotan los búferes. Cuando un dispositivo de red de la recepción se agotan los búferes, un dispositivo de red está intentando volver a ensamblar un paquete fragmentado pero no dispone de recursos para almacenar y reassume el paquete.

La fragmentación se puede percibir como una operación negativo, pero el soporte técnico para la fragmentación es necesaria para conectar varias redes a través de Internet.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Ventajas y las consecuencias de modificar la MTU

Como regla general, aumentar la MTU puede crear una red más eficaz. Todos los paquetes que se transmiten tiene información de encabezado adicional que se agrega al paquete original. Más paquetes significa encabezado más sobrecarga y la red como resultado es menos eficaz.

Por ejemplo, el tamaño del encabezado Ethernet es 14 bytes más de una herramienta 4 bytes marco Check Sequence (FCS) para garantizar la coherencia del marco. Si se envía un paquete de 2000 bytes, se agregan los 18 bytes de sobrecarga de Ethernet en la red. Si el paquete se fragmenta en un paquete de 1500 bytes y un paquete de 500 bytes, cada paquete tendrá 18 bytes del encabezado de Ethernet - o 36 bytes. Mientras que un único paquete de 2000 bytes solo tendría un encabezado de Ethernet de 18 bytes.

Es importante tener en cuenta que aumentar la MTU en sí mismo no necesariamente creará una red más eficaz. Si una aplicación envía solo los paquetes de 500 bytes, la sobrecarga de encabezado mismo existirá si la MTU es 1500 bytes o 9000 bytes. Para la red sea más eficaz, a continuación, lo también debe usar mayores tamaños de paquetes que están en relación con la MTU.

#### <a name="azure-and-vm-mtu"></a>Azure y la MTU de la máquina virtual

La MTU de máquinas virtuales de Azure predeterminada es de 1500 bytes. La pila de red Virtual de Azure intentará fragmento de un paquete en bytes 1400. Sin embargo, la pila de red Virtual de Azure permite paquetes hasta 2006 bytes cuando se establece el bit "No desfragmentar" en el encabezado IP.

Es importante tener en cuenta que esta fragmentación no implica que la pila de red Virtual de Azure es inherentemente ineficaz porque divide los paquetes en bytes 1400 mientras las máquinas virtuales tienen una MTU de 1500. La realidad es que un porcentaje elevado de paquetes de red es mucho menor que 1400 bytes o 1500 bytes.

#### <a name="azure-and-fragmentation"></a>Azure y fragmentación

Pila de red Virtual de Azure hoy en día está configurado para quitar "Desorden de fragmentos" - lo que significa que los paquetes fragmentados que no llegan a su orden original fragmentado. Estos paquetes se quitan principalmente debido a una vulnerabilidad de seguridad de red que se anunció en noviembre de 2018 llamado FragmentStack.

FragmentSmack es un defecto de la manera en que el kernel de Linux administra el reensamblado de paquetes fragmentados de IPv4 e IPv6. Un atacante remoto podría utilizar este defecto desencadenador reensamblado de fragmento costoso las operaciones, que conducen a un aumento de CPU y una denegación de servicio en el sistema de destino.

#### <a name="tune-the-mtu"></a>Optimizar la MTU

Máquinas virtuales de Azure admiten una MTU puede configurar igual que cualquier otro sistema operativo. Sin embargo, debe considerarse la fragmentación que se produce dentro de Azure y se detalla anteriormente, al configurar la MTU.

Azure no anima a los clientes a aumentar sus MTU VM. Esta discusión se pretende explicar detalladamente cómo Azure implementa MTU y realiza la fragmentación de hoy en día.

> [!IMPORTANT]
>Aumentar la MTU no se ha mostrado para mejorar el rendimiento y podría tener un impacto negativo en el rendimiento de la aplicación.
>
>

#### <a name="large-send-offload-lso"></a>Descarga de envío grande (LSO)

Descarga de envío grande (LSO) puede mejorar el rendimiento de red mediante la descarga de la segmentación de los paquetes al adaptador de Ethernet. Con LSO habilitada, la pila TCP/IP creará un paquete TCP grande y, a continuación, enviar al adaptador de Ethernet para la segmentación antes de reenviarlo. La ventaja de LSO es que puede liberar la CPU desde la segmentación de los paquetes en el tamaño de los paquetes que se ajusta a la MTU y descarga ese procesamiento a la interfaz Ethernet donde se realiza en hardware. Puede encontrar más información acerca de las ventajas de LSO en [rendimiento en la documentación del adaptador de red de Microsoft](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

Cuando LSO está habilitada, los clientes de Azure pueden ver los tamaños de marco grande cuando las capturas de paquetes de rendimiento. Estos tamaños de marco grande pueden provocar que algunos clientes para creer que la fragmentación o un gigantes que MTU se está usando cuando no está. Con LSO, el adaptador ethernet puede anunciar un MSS mayor a la pila TCP/IP con el fin de crear un paquete TCP mayor. Este marco no segmentado completo, a continuación, se reenvía al adaptador de Ethernet y sería visible en una captura de paquetes puede realizada en la máquina virtual. Sin embargo, el paquete se divide en muchos marcos más pequeños que el adaptador Ethernet según MTU del adaptador de Ethernet.

### <a name="tcpmss-window-scaling-and-pmtud"></a>Escala de ventana/MSS de TCP y PMTUD

#### <a name="explanation-of-tcp-mss"></a>Explicación de MSS de TCP

Tamaño máximo de segmento (MSS) de TCP es una configuración que se pretende establecer el tamaño máximo de segmento TCP para evitar la fragmentación de los paquetes TCP. Sistemas operativos normalmente establecerá MSS como MSS = MTU - tamaño de encabezado TCP & IP (20 bytes o un total de 40 bytes). Por lo tanto, una interfaz con una MTU de 1500 tendrá un MSS de 1460. Sin embargo, el MSS, es configurable.

Esta configuración se acepta en el protocolo de enlace de tres vías TCP cuando se configura una sesión TCP entre un origen y destino. Un valor de MSS de ambos lados envían y el menor de los dos se usa para la conexión TCP.

Dispositivos de red intermedios, como puertas de enlace de VPN, incluida la puerta de enlace de VPN de Azure, tienen la posibilidad de ajustar la MTU independiente del origen y destino para garantizar un rendimiento óptimo de red. Por lo tanto, debe tenerse en cuenta que la MTU de origen y de destino por sí solo no es los factores en el valor real de MSS único.

#### <a name="explanation-of-path-mtu-discovery-pmtud"></a>Explicación de la detección MTU de ruta de acceso (PMTUD)

Mientras se negocia MSS, puede no indicar el MSS real que se pueden usar como otros dispositivos de red en la ruta de acceso entre el origen y destino puede tener un valor MTU menor que el origen y destino. En este caso, el dispositivo cuyo MTU es menor que el paquete de quitar el paquete y devolver un mensaje de protocolo de mensajes de Control de Internet (ICMP) fragmentación necesaria (tipo 3, 4 de código) que contiene su MTU. Este mensaje ICMP permite al host de origen reducir su MTU de la ruta de acceso adecuadamente. El proceso se denomina detección de MTU de la ruta de acceso.

El proceso de PMTUD es inherentemente ineficaz y tiene implicaciones de rendimiento de la red. Cuando se envían los paquetes que superen una MTU de las rutas de acceso de red, se deben retransmitir dichos paquetes con un MSS inferior. Si el remitente no recibe el paquete ICMP fragmentación necesaria, quizás debido a un servidor de seguridad de red en la ruta de acceso (conocido comúnmente como negras PMTUD), a continuación, el remitente no sabe que necesita para reducir el MSS y le continuamente retransmitir el paquete. Por este motivo, no se recomienda aumentar la MTU de la máquina virtual de Azure.

#### <a name="vpn-considerations-with-mtu"></a>Consideraciones de VPN con MTU

Los clientes que usan las máquinas virtuales que realizan la encapsulación (por ejemplo, IPSec VPN) pueden tener implicaciones adicionales para el tamaño de paquete y MTU. Las redes privadas virtuales agregar encabezados adicionales se agregarán al paquete original, por tanto, aumentar el tamaño de paquete y que requieren un MSS más pequeño.

La recomendación actual de Azure es establecer el bloqueo MSS de TCP a 1350 bytes y la interfaz de túnel MTU 1400. Puede encontrar más información en el [VPN de dispositivos y la página de parámetros de IPSec/IKE](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Latencia, tiempo de ida y vuelta y ajuste de escala de ventana TCP

#### <a name="latency-and-round-trip-time"></a>Tiempo de ida y vuelta y latencia

Latencia de red se rige por la velocidad de la luz en una red de fibra óptica. La realidad es, rendimiento de la red de TCP también está regidos eficazmente (prácticos máximos) debido a la hora de ida y vuelta (RTT) entre dos dispositivos de red.

| | | | |
|-|-|-|-|
|Enrutar|Distancia|Tiempo unidireccional|Tiempo de ida y vuelta (RTT)|
|Nueva York a San Francisco|4,148 km|21 ms|42 ms|
|Nueva York a Londres|5,585 km|28 ms|56 ms|
|Nueva York a Sydney|15,993 km|80 ms|160 ms|

Esta tabla muestra la distancia lineal entre dos ubicaciones, sin embargo, en las redes, la distancia es normalmente mayor que la distancia lineal. Es una fórmula sencilla para calcular el RTT mínimo tal y como se rige por la velocidad de la luz: RTT mínimo = 2 * (kilómetros de distancia o la velocidad de propagación).

Un valor de 200 estándar se puede usar para acelerar el proceso de propagación: el valor es que la distancia en metros light viaja en 1 milisegundo.

En el ejemplo de Nueva York a San Francisco, es 4,148 km distancia lineal. RTT mínimo = 2 * (4,148 / 20). La salida de la ecuación estará en milisegundos.

Como la distancia entre dos ubicaciones física es una realidad fija, si el rendimiento de red máximo es necesario, a continuación, la opción más lógica consiste en seleccionar destinos con la menor distancia entre ellos. En segundo lugar, se pueden tomar decisiones de diseño de la red virtual para optimizar la ruta de acceso de tráfico y reducir la latencia. Se describen estas consideraciones de red virtual en la siguiente sección de consideraciones de diseño de red.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>Efectos en tiempo de ida y vuelta y latencia de TCP

Ida y vuelta (RTT) de tiempo tiene un efecto directo en el máximo rendimiento TCP. El protocolo TCP tiene un concepto de tamaño de la ventana. El tamaño de ventana es la cantidad máxima de tráfico que se puede enviar a través de una conexión TCP antes de que el remitente debe recibir la confirmación de receptor. Si el MSS de TCP está establecido en 1460 y se establece el tamaño de ventana TCP y 65535, a continuación, el remitente puede enviar 45 paquetes antes de debe recibir la confirmación de receptor. Si no se recibe confirmación retransmitirá el remitente. En este ejemplo, el tamaño de ventana TCP / MSS de TCP = paquetes enviados. O 65535 / 1460 se redondea hasta 45.

Este estado "en espera de confirmación", como un mecanismo para crear una entrega confiable de datos, es lo que provoca eficazmente RTT afectar al rendimiento de TCP. Cuanto más tiempo en que el remitente espera de confirmación, cuanto más tiempo también debe esperar antes de enviar más datos.

La fórmula para calcular el rendimiento máximo de una única conexión TCP es como sigue: Tamaño de ventana / (latencia RTT en milisegundos / 1000) = máximo en bytes por segundo. En la tabla siguiente se da formato en Megabytes para mejorar la legibilidad y se muestra el máximo en megabytes por segundo de una única conexión TCP.

| | | | |
|-|-|-|-|
|Tamaño de ventana TCP en Bytes|Latencia RTT<br/>en milisegundos|Máxima<br/>Megabytes por segundo|Máxima<br/> Megabits por segundo|
|65535|1|65.54|524.29|
|65535|30|2.18|17.48|
|65535|60|1,09|8.74|
|65535|90|.73|5.83|
|65535|120|.55|4.37|

Si no hay pérdida de paquetes, reducirá el rendimiento máximo de una conexión TCP mientras que el remitente retransmite datos que ya ha enviado.

#### <a name="explanation-of-tcp-window-scaling"></a>Explicación de ajuste de escala de ventana TCP

Ajuste de escala de ventana TCP es un concepto que aumenta dinámicamente el tamaño de ventana TCP permitir que los datos más para enviarse antes de solicitar una confirmación. En nuestro ejemplo anterior, se enviarían 45 paquetes antes de que se requiere una confirmación. Si el número de paquetes que se envían antes de que se incrementa una confirmación, a continuación, el rendimiento máximo de TCP también aumenta al reducir el número de veces que un remitente está esperando la confirmación.

El rendimiento TCP se muestra en una tabla sencilla a continuación:

| | | | |
|-|-|-|-|
|Tamaño de ventana TCP<br/>en Bytes|RTT latencia en milisegundos|Máxima<br/>Megabytes por segundo|Máxima<br/> Megabits por segundo|
|65535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Sin embargo, el valor del encabezado TCP para el tamaño de ventana TCP tiene sólo 2 bytes, lo que significa que el valor máximo para una ventana de recepción es 65535. Con el fin de aumentar el tamaño máximo de la ventana, se introdujo un Factor de escala de ventana TCP.

El factor de escala es también una configuración que se puede configurar en un sistema operativo. La fórmula para calcular el tamaño de ventana TCP mediante factores de escala es como sigue: Tamaño de ventana TCP = tamaño de ventana TCP en Bytes \* (2 ^ Factor de escala). Si el Factor de escala de ventana es 3 y el tamaño de ventana de 65535, el cálculo es como sigue: 65535 \* (2 ^ 3) = 262,140 bytes. Un Factor de escala de 14 da como resultado un tamaño de ventana TCP 14 (el desplazamiento máximo permitido), entonces el tamaño de ventana TCP será 1,073,725,440 bytes (8,5 gigabits).

#### <a name="support-for-tcp-window-scaling"></a>Compatibilidad con ajuste de escala de ventana TCP

Windows tiene la capacidad de establecer diferentes factores de escala en un por tipo de conexión - hay varias clases de conexiones (datacenter, internet y así sucesivamente). Puede ver la clasificación de conexión de escala de ventana con el comando de powershell Get-NetTCPConnection.

```powershell
Get-NetTCPConnection
```

Puede ver los valores de cada clase con el comando de powershell Get-NetTCPSetting.

```powershell
Get-NetTCPSetting
```

El tamaño de ventana TCP inicial y el Factor de escala de TCP se pueden establecer en Windows mediante el comando de powershell Set-NetTCPSetting. Puede encontrar más información en el [conjunto NetTCPSetting página](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

```powershell
Set-NetTCPSetting
```

La configuración de TCP eficaz para AutoTuningLevel es los siguientes.

| | | | |
|-|-|-|-|
|AutoTuningLevel|Factor de escala|Multiplicador de escala|Fórmula para<br/>calcular el tamaño máximo de ventana|
|Disabled|None|None|Tamaño de ventana|
|Restringido|4|2^4|Tamaño de la ventana * (2 ^ 4)|
|Muy restringido|2|2^2|Tamaño de la ventana * (2 ^ 2)|
|Normal|8|2^8|Tamaño de la ventana * (2 ^ 8)|
|Experimental|14|2^14|Tamaño de la ventana * (2 ^ 14)|

Aunque estas opciones siguen más probabilidades de afectar al rendimiento de TCP, debe tenerse muchos otros factores a través de Internet, fuera del control de Azure, también pueden afectar a rendimiento de TCP.

#### <a name="increase-mtu-size"></a>Aumentar el tamaño de MTU

¿Una pregunta lógica es "puede aumentar la MTU de aumentar el rendimiento de TCP como una MTU mayor significa una mayor MSS"? La respuesta sencilla es, probablemente no. Como se ha explicado, hay ventajas y desventajas al tamaño de paquete que se pueden aplicables más allá de simplemente el tráfico TCP. Como se dijo anteriormente, los factores más importantes que afectan al rendimiento de TCP es RTT, pérdida de paquetes y tamaño de ventana TCP.

> [!IMPORTANT]
> Azure no recomienda que los clientes de Azure modifique el valor MTU predeterminada en máquinas virtuales.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Las redes aceleradas y escala en lado de recepción

#### <a name="accelerated-networking"></a>Redes aceleradas

Las funciones de red de máquina virtual históricamente han sido CPU considerable en el invitado de máquina virtual y el Host de hipervisor. El host de CPU - incluyendo toda la red Virtual encapsulación/de-capsulation procesa todos los paquetes que transita a través del host en el software. Por lo tanto, más el tráfico que pasa por el host y, a continuación, mayor será la carga de CPU. Y, si la CPU del host está realizando otras operaciones, a continuación, que también afectará el rendimiento de red y la latencia. Se ha solucionado este problema a través de redes aceleradas.

Redes aceleradas proporcionan la latencia de red muy baja coherente a través de tecnologías como SR-IOV y hardware programable interna de Azure. Moviendo gran parte de la pila de redes definidas por software de Azure desactivado las CPU y en SmartNICs basada en FPGA, calcular los reclama ciclos de las aplicaciones de usuario final, colocando menos carga en la máquina virtual, se reduce la vibración y las incoherencias en la latencia. En otras palabras, el rendimiento puede ser más determinista.

Redes aceleradas consigue mejoras de rendimiento al permitir que la máquina virtual invitada omitir el host y establecer una ruta de datos directamente con un host SmartNIC. Ventajas de Accelerated Networking son:

- **Menor latencia / más paquetes por segundo (pps)**: al quitarse el conmutador virtual de la ruta de acceso de datos, se elimina el tiempo que los paquetes pasan en el host para el procesamiento de las directivas y se aumenta el número de paquetes que se pueden procesar dentro de la máquina virtual.

- **Inestabilidad reducida**: el procesamiento del conmutador virtual depende de la cantidad de directivas que deben aplicarse y la carga de trabajo de la CPU que se encarga del procesamiento. Al descargarse la aplicación de directivas en el hardware, se elimina esa variabilidad, ya que los paquetes se entregan directamente a la máquina virtual y se elimina el host de la comunicación de la máquina virtual, así como todas las interrupciones de software y los cambios de contexto.

- **Reducir el uso de CPU**: el pasar por alto el conmutador virtual en el host conlleva una disminución de la utilización de la CPU para procesar el tráfico de red.

Redes aceleradas deben habilitarse explícitamente en una base por máquina virtual. Instrucciones para habilitar Accelerated Networking en una máquina virtual están disponibles en el [crear una máquina virtual Linux con Accelerated Networking página](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

#### <a name="receive-side-scaling-rss"></a>Recepción (RSS) de escalado

Escala en lado de recepción es una tecnología de controlador de red que distribuye la recepción de tráfico de red de forma más eficaz mediante la distribución de procesamiento de recepción a través de varias CPU en un sistema multiprocesador. En términos sencillos, RSS permite que un sistema procesar una mayor cantidad de tráfico recibido porque usa todas las CPU disponibles en lugar de uno. Encontrará una explicación más técnica de RSS en la [Introducción a la página de escala de recepción](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

RSS es necesario para lograr el máximo rendimiento cuando Accelerated Networking está habilitada en una máquina virtual. También puede haber ventajas del uso de RSS en máquinas virtuales que no tienen redes aceleradas habilitadas. Información general sobre cómo determinar si RSS está habilitado y configuración para habilitarlo puede encontrarse en el [optimizar el rendimiento de red para máquinas virtuales de Azure página](http://aka.ms/FastVM).

### <a name="tcp-time-wait-and-time-wait-assassination"></a>Tiempo y espera TCP tiempo de espera Assassination

Otro problema común que afecta al rendimiento de red y la aplicación es la configuración de tiempo de espera de TCP. En las máquinas virtuales ocupadas que son de apertura y cierre muchos sockets, ya sea como un cliente o servidor (puerto de origen IP:Source + IP:Destination el puerto de destino), durante el funcionamiento normal del TCP, un socket determinado puede acabar en un estado de tiempo de espera para una cantidad considerable de tiempo. Este estado de "tiempo de espera", está pensado para permitir que los datos adicionales que se entregue en un socket antes de cerrarla. Por lo tanto, las pilas de TCP/IP generalmente impedir la reutilización de un socket colocando en modo silencioso los paquetes TCP SYN clientes.

Esta cantidad de tiempo de un socket está en estado de espera es configurable, pero puede oscilar entre 30 segundos y 240 segundos de tiempo. Los sockets son un recurso finito, y el número de sockets que puede usarse en un momento dado se pueden configurar (el número generalmente encuentra aproximadamente 30.000 posibles sockets). Si se agota este número, o clientes y servidores tienen la configuración de espera de hora no coincidentes y una máquina virtual intenta volver a usar un socket en estado en tiempo de espera, a continuación, las nuevas conexiones se producirá un error como paquetes TCP SYN son quitados.

Normalmente, el valor de intervalo de puertos para los sockets de salida, así como la configuración de tiempo de espera de TCP y reutilización del socket son configurables dentro de la pila TCP/IP de un sistema operativo. Cambiar estos números pueden mejorar la escalabilidad, pero según el escenario, podría provocar problemas de interoperabilidad y debe cambiarse con precaución.

Una funcionalidad denominada "tiempo de espera Assassination se introdujo para solucionar este problema, limitación de escala. Tiempo de espera Assassination permite un socket para reutilizarse en determinados escenarios, como cuando el número de secuencia en el paquete IP de la nueva conexión supera el número de secuencia del último paquete de la conexión anterior. En este caso, el sistema operativo permitirá que se establezca la conexión nueva (acepta el nuevo SYN ACK) y Forzar cierre la conexión anterior que estaba en tiempo de estado de espera. Esta funcionalidad se admite en máquinas virtuales Windows en Azure hoy mismo y debe investigarse compatibilidad con otras máquinas virtuales dentro de los clientes de Azure con los proveedores del sistema operativo correspondientes.

Documentación sobre la configuración de la configuración de tiempo de espera de TCP y el intervalo de puertos de origen está disponible en el [configuración que se puede modificar a la página de mejorar el rendimiento de red](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Factores de red virtuales que pueden afectar al rendimiento

### <a name="vm-maximum-outbound-throughput"></a>Rendimiento máximo saliente de máquina virtual

Azure ofrece una variedad de tamaños y tipos de máquinas virtuales, cada uno con una combinación diferente de funcionalidades de rendimiento. Un tal capacidad de rendimiento es red rendimiento (o ancho de banda) medido en megabits por segundo (Mbps). Dado que las máquinas virtuales se hospedan en hardware compartido, la capacidad de red debe compartirse equitativamente entre las máquinas virtuales que compartan el mismo hardware. A las máquinas virtuales más grandes se les asigna relativamente más ancho de banda que las más pequeñas.

El ancho de banda de red asignado a cada máquina virtual se mide en el tráfico de salida de la máquina virtual. Todo el tráfico de red que deja la máquina virtual se cuenta para el límite asignado, independientemente del destino. Por ejemplo, si una máquina virtual tiene un límite de 1000 Mbps, ese límite se aplica si el tráfico saliente está destinado a otra máquina virtual en la misma red virtual o fuera de Azure.
La entrada no se mide o no está directamente limitada. Sin embargo, hay otros factores, como los límites de la CPU y de almacenamiento, que pueden afectar la capacidad de una máquina virtual de procesar los datos entrantes.

Redes aceleradas son una característica diseñada para mejorar el rendimiento de red, incluida la latencia, rendimiento y uso de CPU. Mientras que redes aceleradas pueden mejorar el rendimiento de una máquina virtual, puede hacerlo solo hasta la máquina virtual de ancho de banda asignado.

Las máquinas virtuales de Azure deben tener una interfaz de red, pero pueden tener varias conectadas a ellas. El ancho de banda asignado a una máquina virtual es la suma de todo el tráfico saliente en todas las interfaces de red conectadas a una máquina virtual. En otras palabras, el ancho de banda asignado es por máquina virtual, independientemente de la cantidad de interfaces de red conectadas a la máquina virtual.
 
Rendimiento de salida esperado y el número de interfaces de red admite cada tamaño de máquina virtual se detalla aquí. Para ver el rendimiento máximo, seleccione un tipo, como de uso General, a continuación, seleccione una serie de tamaño en la página resultante, por ejemplo, la serie Dv2. Cada serie tiene una tabla con las redes de las especificaciones de la última columna, titulada, Nº máx. NIC / rendimiento de red (Mbps) esperado.

El límite de rendimiento se aplica a la máquina virtual. El rendimiento no se ve afectado por los siguientes factores:

- **Número de interfaces de red**: El límite de ancho de banda es acumulativo de todo el tráfico saliente desde la máquina virtual.

- **Las redes aceleradas**: Aunque la característica puede ser útil para lograr el límite publicado, no cambia el límite.

- **Destino del tráfico**: Todos los destinos se cuentan para el límite de salida.

- **Protocolo**: Todo el tráfico saliente a través de todos los protocolos cuenta para el límite.

Un [tabla de ancho de banda máximo por tipo de máquina virtual puede encontrarse al visitar esta página](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) y haga clic en el tipo de máquina virtual correspondiente. En cada página de tipo, una tabla mostrará las NIC máximas y el ancho de banda de red esperado máximo.

Para obtener más información sobre el ancho de banda de red de máquina virtual puede encontrarse en [ancho de banda de red de máquina Virtual](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Consideraciones de rendimiento de Internet

Como se describe en este artículo, los factores en Internet y fuera del control de Azure pueden afectar al rendimiento de la red. Estos factores son:

- **Latencia**: El tiempo de ida y vuelta entre dos destinos puede verse afectado por problemas en redes intermedias, no realiza la distancia "más corta" ruta de acceso posible poco óptimos emparejamiento rutas de acceso y el tráfico

- **Pérdida de paquetes**: Pérdida de paquetes puede deberse a la congestión de la red, problemas de la ruta de acceso física y los dispositivos de red de bajo rendimiento

- **Tamaño y fragmentación de MTU**: Fragmentación a lo largo de la ruta de acceso puede dar lugar a retrasos en la llegada de los datos o los paquetes que llegan desordenados, que pueden afectar a la entrega de paquetes

Traceroute es una buena herramienta para medir las características de rendimiento de red (por ejemplo, pérdida de paquetes y latencia) a lo largo de cada ruta de acceso de red entre un dispositivo de origen y destino.

### <a name="network-design-considerations"></a>Consideraciones de diseño de red

Junto con las consideraciones anteriores, la topología de una red Virtual puede afectar al rendimiento de la red Virtual. Por ejemplo, un concentrador y radio diseño que el tráfico de redes de direccionamiento todo el mundo para una red virtual de concentrador solo se presentan la latencia de red y, por tanto, afectar al rendimiento de red general. De forma similar, el número de dispositivos de red que atraviesa el tráfico de red también puede afectar a la latencia total. Por ejemplo, en un diseño de concentrador y radio, si el tráfico pasa a través de un radio un dispositivo Virtual de concentrador y de aplicación Virtual de red antes de tránsito a Internet, a continuación, latencia puede introducir las aplicaciones virtuales de red.

### <a name="azure-regions-virtual-networks-and-latency"></a>Regiones de Azure, las redes virtuales y latencia

Regiones de Azure se componen de varios centros de datos que existen dentro de un área geográfica general. Estos centros de datos pueden no estar físicamente entre sí y en algunos casos pueden separarse con hasta 10 kilómetros. La red Virtual es una lógica superpuesta sobre red de centro de datos físicos de Azure y una red Virtual no implica cualquier topología de red específico dentro del centro de datos. Por ejemplo, VM A y B de la VM se encuentran en la misma red Virtual y subred, pero pueden ser de diferentes bastidores, filas o incluso los centros de datos. Se pueden separar por metros de cable de fibra óptica o kilómetros de cable de fibra óptica. Esta realidad puede introducir latencia variable (diferencia de pocos milisegundos) entre diferentes máquinas virtuales.

Esta selección de ubicación geográfica y, por tanto, la latencia entre dos máquinas virtuales, pueden influir en la configuración de conjuntos de disponibilidad y las zonas de disponibilidad, sin embargo, la distancia entre los centros de datos en una región es específica de la región e influido principalmente por topología de centro de datos en la región.

### <a name="source-nat-port-exhaustion"></a>Agotamiento de puertos NAT de origen

Una implementación de Azure puede comunicarse con puntos de conexión fuera de Azure en la red pública de Internet o el espacio de IP públicas. Cuando una instancia inicia esta conexión de salida, Azure asigna dinámicamente la dirección IP privada a una dirección IP pública. Una vez creada esta asignación, el tráfico de retorno de este flujo originado de salida también puede comunicarse con la dirección IP privada donde se originó el flujo.

Para todas las conexiones salientes, el equilibrador de carga de Azure debe mantener esta asignación para un período de tiempo. Con la naturaleza multiempresa de Azure, mantener esta asignación para cada flujo de salida para cada máquina virtual puede consumir muchos recursos. Por lo tanto, hay límites en los que se establecen y según la configuración de la red Virtual de Azure. O indicados de forma más precisa - una máquina virtual de Azure solo puede realizar un cierto número de conexiones salientes en un momento dado. Cuando se agotan estos límites, se evitará realicen las conexiones salientes aún más la máquina virtual de Azure.

Este comportamiento es, sin embargo, puede configurar. Para obtener más información sobre [SNAT y SNAT puerto agotamiento], vea [en este artículo](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>Medición del rendimiento de red en Azure

Un número de los valores máximos de rendimiento en este artículo está relacionados con la latencia de red / ida y vuelta (RTT) de tiempo entre dos máquinas virtuales. Esta sección proporciona algunas sugerencias para probar la latencia/RTT, así como el rendimiento de TCP y rendimiento de la red de máquina virtual. Se pueden ajustar los valores de TCP/IP y la red que se ha explicado anteriormente y prueba de rendimiento mediante las técnicas descritas a continuación. Los valores de latencia, MTU, MSS y tamaño de la ventana se pueden usar en los cálculos enumerados anteriormente y los valores máximos teóricos pueden compararse con valores reales observados durante las pruebas.

### <a name="measure-round-trip-time-and-packet-loss"></a>Tiempo de ida y vuelta de medida y pérdida de paquetes

El rendimiento de TCP depende en gran medida RTT y pérdida de paquetes. La manera más sencilla de medir el RTT y pérdida de paquetes es mediante la utilidad de ping disponible en Windows y Linux. La salida de ping mostrará min/max/promedio latencia entre un origen y destino, así como pérdida de paquetes. Ping usa el protocolo ICMP de forma predeterminada. Para probar TCP RTT, a continuación, PsPing se puede usar. Obtener más información sobre PsPing está disponible en [este vínculo](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>Medir el rendimiento real de una conexión TCP

NTttcp es una herramienta que se usa para probar el rendimiento de TCP de una VM de Windows o Linux. Se puede modificar la configuración de TCP distintos y las ventajas se comprueba empleando NTttcp. Para obtener más información acerca de NTttcp puede encontrarse en los vínculos siguientes.

- [Ancho de banda y rendimiento de las pruebas (NTttcp)](https://aka.ms/TestNetworkThroughput)

- [Utilidad de NTttcp](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Medida de ancho de banda real de una máquina virtual

Las pruebas de rendimiento de diferentes tipos de máquinas virtuales, redes aceleradas y así sucesivamente, se pueden probar mediante una herramienta denominada Iperf, también está disponible en Linux y Windows. Iperf puede usar TCP o UDP para probar el rendimiento general de la red. Pruebas de rendimiento TCP mediante Iperf están influenciadas por los factores descritos en este artículo (RTT, latencia etc.). Por lo tanto, UDP puede arrojar resultados mejores para simplemente realizar pruebas de rendimiento máximo.

A continuación se puede encontrar información adicional:

- [Solución de problemas de rendimiento de la red Expressroute](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Validación del rendimiento de la VPN en una red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Detectar comportamientos TCP ineficaces

Los clientes de Azure pueden obtener los paquetes TCP con indicadores de TCP (SACK, DUP ACK, retransmisión y retransmisión rápida) en las capturas de paquetes que pueden indicar problemas de rendimiento de red. Estos paquetes indican específicamente las ineficiencias de la red como resultado la pérdida de paquetes. Sin embargo, pérdida de paquetes no es necesariamente debido a problemas de rendimiento de Azure. Problemas de rendimiento podrían ser el resultado de la aplicación, sistema operativo u otros problemas que podrían no estar directamente relacionada con la plataforma Azure. También es importante tener en cuenta que algunos retransmisión o confirmaciones duplicadas en una red es normal, protocolos TCP se han diseñado para ser confiable. Y la evidencia de estos paquetes TCP en una captura de paquetes no indica necesariamente un problema de red sistémicos a menos que sean excesivos.

Sin embargo, debe indicarse claramente que estos tipos de paquetes son las indicaciones que el rendimiento TCP no está consiguiendo su rendimiento máximo: por razones descritas en otras secciones.
