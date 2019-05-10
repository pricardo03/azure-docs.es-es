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
ms.date: 04/02/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: d0124d6656167af3942e0d054b4e1fa7a2b48e8b
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410044"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP optimizar el rendimiento de las máquinas virtuales de Azure

Este artículo describen técnicas de optimización de rendimiento de TCP/IP comunes y algunos aspectos a tener en cuenta cuando se usan para las máquinas virtuales que se ejecutan en Azure. Se proporciona una introducción básica de las técnicas y explore cómo pueden optimizarse.

## <a name="common-tcpip-tuning-techniques"></a>Técnicas de optimización de TCP/IP comunes

### <a name="mtu-fragmentation-and-large-send-offload"></a>MTU, la fragmentación y descarga de envío grande

#### <a name="mtu"></a>MTU

La unidad de transmisión máxima (MTU) es el marco de tamaño más grande (paquete), especificado en bytes, que pueden enviarse a través de una interfaz de red. La MTU es un valor configurable. El valor predeterminado MTU utiliza en máquinas virtuales de Azure y la configuración predeterminada en la mayoría de los dispositivos de red global, es 1.500 bytes.

#### <a name="fragmentation"></a>Fragmentación

La fragmentación se produce cuando se envía un paquete que supera la MTU de una interfaz de red. La pila TCP/IP interrumpirá el paquete en partes más pequeñas (fragmentos) que se ajustan a la MTU de la interfaz. La fragmentación se produce en el nivel de IP y es independiente del protocolo subyacente (por ejemplo, TCP). Cuando se envía un paquete de 2.000 bytes a través de una interfaz de red con una MTU de 1.500, el paquete se divide en un paquete de 1.500 bytes y un paquete de 500 bytes.

Dispositivos de red en la ruta de acceso entre un origen y destino pueden eliminar cualquier los paquetes que superan la MTU o fragmentar el paquete en partes más pequeñas.

#### <a name="the-dont-fragment-bit-in-an-ip-packet"></a>El bit en un paquete IP no fragmentar

El bit no fragmentar (DF) es una marca en el encabezado de protocolo IP. El bit DF indica que los dispositivos de red en la ruta de acceso entre el remitente y receptor deben no fragmentar el paquete. Se puede establecer este bit por diversos motivos. (Consulte la sección "Detección de MTU de la ruta de acceso" de este artículo para obtener un ejemplo). Cuando un dispositivo de red recibe un paquete con el conjunto de bits no fragmentar y ese paquete supera la MTU de la interfaz del dispositivo, es el comportamiento estándar para que el dispositivo quitar el paquete. El dispositivo envía un mensaje ICMP fragmentación necesaria en el origen del paquete.

#### <a name="performance-implications-of-fragmentation"></a>Implicaciones de rendimiento de fragmentación

La fragmentación puede afectar al rendimiento negativo. Una de las razones principales para el efecto en el rendimiento es el impacto de CPU/memoria de la fragmentación y reensamblado de paquetes. Cuando un dispositivo de red necesita un paquete de fragmentos, tendrá que asignar recursos de CPU/memoria para llevar a cabo la fragmentación.

Lo mismo sucede cuando se vuelve a ensamblar el paquete. El dispositivo de red tiene que almacenar todos los fragmentos hasta que se reciben para reensamblarlas en el paquete original. Este proceso de fragmentación y reensamblado también puede hacer que la latencia.

La otra implicación de posibles negativo en el rendimiento de fragmentación es que los paquetes fragmentados pueden llegar desordenados. Cuando se reciben los paquetes sin orden, algunos tipos de dispositivos de red pueden quitarlos. Cuando esto ocurre, el paquete completo se tiene que ser retransmitidos.

Fragmentos se quitan normalmente con dispositivos de seguridad, como los firewalls de red o cuando un dispositivo de red de la recepción se agotan los búferes. Cuando un dispositivo de red de la recepción se agotan los búferes, un dispositivo de red está intentando volver a ensamblar un paquete fragmentado pero no dispone de recursos para almacenar y reassume el paquete.

La fragmentación puede verse como una operación negativo, pero el soporte técnico de fragmentación es necesaria cuando se conecta a través de internet varias redes.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Ventajas y las consecuencias de modificar la MTU

Por lo general, puede crear una red más eficaz si aumenta la MTU. Todos los paquetes que se transmiten tiene información de encabezado que se agrega al paquete original. Cuando la fragmentación crea más paquetes, hay encabezado más sobrecarga y esto hace que la red menos eficiente.

Este es un ejemplo. El tamaño del encabezado Ethernet es 14 bytes más de una secuencia de comprobación de marcos de 4 bytes para garantizar la coherencia del marco. Si se envía un paquete de 2.000 bytes, se agregan los 18 bytes de sobrecarga de Ethernet en la red. Si el paquete se fragmenta en un paquete de 1.500 bytes y un paquete de 500 bytes, cada paquete tendrá 18 bytes del encabezado de Ethernet, un total de 36 bytes.

Tenga en cuenta que aumentar la MTU no es preciso crear necesariamente una red más eficaz. Si una aplicación envía solo los paquetes de 500 bytes, la sobrecarga de encabezado mismo existirá si la MTU es 1.500 bytes o 9.000 bytes. La red será más eficaz únicamente si utiliza mayores tamaños de paquetes que se ven afectados por la MTU.

#### <a name="azure-and-vm-mtu"></a>Azure y la MTU de la máquina virtual

La MTU de máquinas virtuales de Azure predeterminado es 1.500 bytes. La pila de red Virtual de Azure intentará fragmento de un paquete en bytes de 1400. Pero la pila de red Virtual le permitirá paquetes 2,006 bytes cuando se establece el bit no fragmentar en el encabezado IP.

Tenga en cuenta que la pila de red Virtual no es intrínsecamente ineficaz porque divide los paquetes en 1.400 bytes, aunque las máquinas virtuales tienen una MTU de 1.500. Un porcentaje elevado de paquetes de red son mucho más pequeño que 1.400 o 1.500 bytes.

#### <a name="azure-and-fragmentation"></a>Azure y fragmentación

Pila de red virtual se configura para quitar "desordenados fragmentos", es decir, los paquetes fragmentados que no llegan a su orden original fragmentado. Estos paquetes se quitan principalmente debido a una vulnerabilidad de seguridad de red que se anunció en noviembre de 2018 llamado FragmentSmack.

FragmentSmack es un defecto de la manera en que el kernel de Linux administra el reensamblado de paquetes fragmentados de IPv4 e IPv6. Un atacante remoto podría utilizar este defecto desencadenador reensamblado de fragmento costoso las operaciones, lo que puede provocar un aumento de CPU y una denegación de servicio en el sistema de destino.

#### <a name="tune-the-mtu"></a>Optimizar la MTU

Puede configurar un valor de MTU de la máquina virtual de Azure, que lo haría en cualquier otro sistema operativo. Pero se debe considerar la fragmentación que se produzca en Azure, se ha descrito anteriormente, cuando se configura una MTU.

No recomendamos a los clientes para aumentar la MTU de la máquina virtual. Esta explicación es ideada para explicar los detalles de cómo Azure implementa MTU y realiza la fragmentación.

> [!IMPORTANT]
>Aumentar la MTU es desconocido para mejorar el rendimiento y podría tener un impacto negativo en el rendimiento de la aplicación.
>
>

#### <a name="large-send-offload"></a>Descarga de envío grande

Descarga de envío grande (LSO) puede mejorar el rendimiento de red mediante la descarga de la segmentación de los paquetes al adaptador de Ethernet. Cuando se habilita LSO, la pila TCP/IP crea un paquete TCP grande y lo envía al adaptador de Ethernet para la segmentación antes de reenviarlo. La ventaja de LSO es que puede liberar la CPU desde la segmentación de los paquetes en tamaños que se ajustan a la MTU y descargar ese procesamiento a la interfaz Ethernet donde se realiza en hardware. Para obtener más información acerca de las ventajas de LSO, vea [que admiten el envío grande descarga](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

Cuando LSO está habilitada, los clientes de Azure podrían ver los tamaños de marco grande al realizar capturas de paquetes. Estos tamaños de marco grande podrían provocar algunos clientes a pensar que se está produciendo la fragmentación o que se está usando una MTU grande cuando no lo está. Con LSO, el adaptador Ethernet puede anunciar un tamaño mayor de máximo de segmento (MSS) a la pila TCP/IP para crear un paquete TCP mayor. Este marco no segmentado completo, a continuación, se reenvía al adaptador de Ethernet y sería visible en una captura de paquetes puede realizada en la máquina virtual. Pero el paquete se se divide en muchos marcos más pequeños por el adaptador de Ethernet, según la MTU del adaptador de Ethernet.

### <a name="tcp-mss-window-scaling-and-pmtud"></a>Escala de ventana de MSS de TCP y PMTUD

#### <a name="tcp-maximum-segment-size"></a>Tamaño máximo de segmento TCP

Tamaño máximo de segmento TCP (MSS) es una configuración que limita el tamaño de los segmentos TCP, lo que evita la fragmentación de los paquetes TCP. Sistemas operativos normalmente utilizará esta fórmula para establecer MSS:

`MSS = MTU - (IP header size + TCP header size)`

El encabezado IP y el encabezado TCP son 20 bytes cada uno, o el total de 40 bytes. Por lo tanto una interfaz con una MTU de 1.500 tendrá un tamaño máximo de 1,460. Pero el MSS es configurable.

Esta configuración es aceptada en el protocolo de enlace de tres vías TCP cuando se configura una sesión TCP entre un origen y un destino. Un valor de MSS de ambos lados envían y el menor de los dos se usa para la conexión TCP.

Tenga en cuenta que la MTU de origen y destino no es los únicos factores que determinan el valor de MSS. Dispositivos de red intermedios, como puertas de enlace VPN, incluido Azure VPN Gateway, pueden ajustar la MTU independientemente de origen y destino para garantizar un rendimiento óptimo de red.

#### <a name="path-mtu-discovery"></a>Ruta de acceso MTU

Se negocia MSS, pero no puede indicar el MSS real que se puede usar. Esto es porque otros dispositivos de red en la ruta de acceso entre el origen y el destino podrían tener un valor MTU menor que el origen y destino. En este caso, el dispositivo cuyo MTU es menor que el paquete quitará el paquete. El dispositivo enviará de vuelta un mensaje ICMP fragmentación necesaria (tipo 3, 4 de código) que contiene su MTU. Este mensaje ICMP permite al host de origen reducir su MTU de la ruta de acceso adecuadamente. El proceso se denomina detección de MTU de la ruta de acceso (PMTUD).

El proceso PMTUD es ineficaz y afecta al rendimiento de la red. Cuando se envían los paquetes que superen la MTU de la ruta de red, los paquetes deben retransmitir con un MSS inferior. Si el remitente no recibe el mensaje ICMP fragmentación necesaria, quizás debido a un servidor de seguridad de red en la ruta de acceso (conocido comúnmente como un *PMTUD negras*), el remitente no sabe que necesita para reducir el MSS y le continuamente retransmitir el paquete. Se trata de por qué no se recomienda aumentar la MTU de la máquina virtual de Azure.

#### <a name="vpn-and-mtu"></a>MTU y VPN

Si usa máquinas virtuales que realizan la encapsulación (por ejemplo, las VPN de IPsec), hay algunas consideraciones adicionales sobre el tamaño de paquete y la MTU. Las VPN de agregar más encabezados en los paquetes, que aumenta el tamaño del paquete y se requiere una menor MSS.

Para Azure, se recomienda que establezca bloqueo MSS de TCP a 1350 bytes y túnel MTU de la interfaz a 1400. Para obtener más información, consulte el [VPN de dispositivos y la página de parámetros de IPSec/IKE](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Latencia, tiempo de ida y vuelta y ajuste de escala de ventana TCP

#### <a name="latency-and-round-trip-time"></a>Tiempo de ida y vuelta y latencia

Latencia de red se rige por la velocidad de la luz en una red de fibra óptica. Rendimiento de la red de TCP eficazmente también se rige por el tiempo de ida y vuelta (RTT) entre dos dispositivos de red.

| | | | |
|-|-|-|-|
|**Route**|**distancia**|**Tiempo unidireccional**|**RTT**|
|Nueva York a San Francisco|4,148 km|21 ms|42 ms|
|Nueva York a Londres|5,585 km|28 ms|56 ms|
|Nueva York a Sydney|15,993 km|80 ms|160 ms|

Esta tabla muestra la distancia lineal entre dos ubicaciones. En las redes, la distancia es normalmente mayor que la distancia lineal. Aquí es una fórmula sencilla para calcular el RTT mínimo tal y como se rige por la velocidad de la luz:

`minimum RTT = 2 * (Distance in kilometers / Speed of propagation)`

Puede usar 200 para la velocidad de propagación. Se trata de la distancia, en metros, que viaja luz en 1 milisegundo.

Tomemos Nueva York a San Francisco como un ejemplo. La distancia lineal es 4,148 km. Conectar ese valor en la ecuación, se obtiene lo siguiente:

`Minimum RTT = 2 * (4,148 / 200)`

El resultado de la ecuación es en milisegundos.

Si desea obtener el mejor rendimiento de red, la opción lógica consiste en seleccionar destinos con la distancia más corta entre ellos. También debe diseñar la red virtual para optimizar la ruta de acceso de tráfico y reducir la latencia. Para obtener más información, consulte la sección "Consideraciones de diseño de red" de este artículo.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>Efectos en tiempo de ida y vuelta y latencia de TCP

Tiempo de ida y vuelta tiene un efecto directo en el máximo rendimiento TCP. En el protocolo TCP, *tamaño de la ventana* es la cantidad máxima de tráfico que se puede enviar a través de una conexión TCP antes de que el remitente debe recibir la confirmación de que el receptor. Si el MSS de TCP está establecido en 1,460 y el tamaño de ventana TCP se establece en 65.535, el remitente puede enviar 45 paquetes antes de que tenga que recibir la confirmación de que el receptor. Si el remitente no recibe la confirmación, se retransmitirá los datos. Esta es la fórmula:

`TCP window size / TCP MSS = packets sent`

En este ejemplo, 65 535 / 1,460 se redondea hasta 45.

Este estado "en espera de confirmación", un mecanismo para garantizar una entrega confiable de datos, es lo que ocasiona el RTT afectar al rendimiento de TCP. Cuanto más tiempo en que el remitente espera de confirmación, cuanto más tiempo debe esperar antes de enviar más datos.

Esta es la fórmula para calcular el rendimiento máximo de una única conexión TCP:

`Window size / (RTT latency in milliseconds / 1,000) = maximum bytes/second`

Esta tabla muestra el máximo número de megabytes por segundo de una única conexión TCP. (Para mejorar la legibilidad, megabytes se usa para la unidad de medida).

| | | | |
|-|-|-|-|
|**Tamaño de ventana TCP (bytes)**|**Latencia RTT (ms)**|**Máximo en megabytes por segundo de rendimiento**|**Máximo megabits/segundo de rendimiento**|
|65 535|1|65.54|524.29|
|65 535|30|2.18|17.48|
|65 535|60|1,09|8.74|
|65 535|90|.73|5.83|
|65 535|120|.55|4.37|

Si se pierden paquetes, se reducirá el rendimiento máximo de una conexión TCP mientras que el remitente retransmite datos que ya ha enviado.

#### <a name="tcp-window-scaling"></a>Ajuste de escala de ventana TCP

Ajuste de escala de ventana TCP es una técnica que aumenta dinámicamente el tamaño de ventana TCP para permitir más datos que deben enviarse antes de solicitar una confirmación. En el ejemplo anterior, se enviarían 45 paquetes antes de que se requiere una confirmación. Si aumenta el número de paquetes que se pueden enviar antes de que se necesita una confirmación, se reducen el número de veces que un remitente está esperando la confirmación, lo que aumenta el rendimiento máximo de TCP.

Esta tabla muestra esas relaciones:

| | | | |
|-|-|-|-|
|**Tamaño de ventana TCP (bytes)**|**Latencia RTT (ms)**|**Máximo en megabytes por segundo de rendimiento**|**Máximo megabits/segundo de rendimiento**|
|65 535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Pero el valor del encabezado TCP para el tamaño de ventana TCP tiene sólo 2 bytes, lo que significa que el valor máximo de una ventana de recepción es de 65.535. Para aumentar el tamaño máximo de la ventana, se introdujo un factor de escala de ventana TCP.

El factor de escala también es una configuración que se puede configurar en un sistema operativo. Esta es la fórmula para calcular el tamaño de ventana TCP mediante el uso de factores de escala:

`TCP window size = TCP window size in bytes \* (2^scale factor)`

Este es el cálculo de un factor de escala de 3 y un tamaño de ventana de 65.535:

`65,535 \* (2^3) = 262,140 bytes`

Un factor de escala de 14 da como resultado un tamaño de ventana TCP de 14 (el desplazamiento máximo permitido). El tamaño de ventana TCP será 1,073,725,440 bytes (8,5 gigabits).

#### <a name="support-for-tcp-window-scaling"></a>Compatibilidad con ajuste de escala de ventana TCP

Windows pueden establecer diferentes factores de escala para diferentes tipos de conexión. (Las clases de las conexiones incluyen centros de datos, internet y así sucesivamente). Usa el `Get-NetTCPConnection` comando de PowerShell para ver el tipo de conexión de la escala de ventana:

```powershell
Get-NetTCPConnection
```

Puede usar el `Get-NetTCPSetting` comando de PowerShell para ver los valores de cada clase:

```powershell
Get-NetTCPSetting
```

Puede establecer el tamaño de ventana TCP inicial y el factor de escala de TCP en Windows mediante el `Set-NetTCPSetting` comando de PowerShell. Para obtener más información, consulte [conjunto NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps).

```powershell
Set-NetTCPSetting
```

Se trata de la configuración de TCP eficaz para `AutoTuningLevel`:

| | | | |
|-|-|-|-|
|**AutoTuningLevel**|**Factor de escala**|**Multiplicador de escala**|**Fórmula para<br/>calcular el tamaño máximo de ventana**|
|Disabled|None|None|Tamaño de ventana|
|Restringido|4|2^4|Tamaño de la ventana * (2 ^ 4)|
|Restringido|2|2^2|Tamaño de la ventana * (2 ^ 2)|
|Normal|8|2^8|Tamaño de la ventana * (2 ^ 8)|
|Experimental|14|2^14|Tamaño de la ventana * (2 ^ 14)|

Esta configuración es más probabilidades de afectar al rendimiento de TCP, pero tenga en cuenta que muchos otros factores a través de internet, fuera del control de Azure, también pueden afectar a rendimiento de TCP.

#### <a name="increase-mtu-size"></a>Aumentar el tamaño de MTU

Debido a una MTU mayor implica una mayor MSS, tal vez se pregunte si aumenta la MTU puede aumentar el rendimiento de TCP. Probablemente no. Existen ventajas y desventajas al tamaño de paquete más allá de simplemente el tráfico TCP. Como se explicó anteriormente, los factores más importantes que afectan al rendimiento de TCP están RTT, pérdida de paquetes y tamaño de ventana TCP.

> [!IMPORTANT]
> No se recomienda que los clientes de Azure cambia el valor MTU predeterminada en máquinas virtuales.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Las redes aceleradas y escala en lado de recepción

#### <a name="accelerated-networking"></a>Redes aceleradas

Las funciones de red de máquina virtual históricamente han sido CPU considerable en la máquina virtual invitada y el host de hipervisor. El host de la CPU, incluidas todas las virtuales red para encapsulación y desencapsulación procesa todos los paquetes que transita a través del host en el software. Por lo que el más tráfico que atraviesa el host, cargar cuanto mayor sea la CPU. Y si la CPU del host está ocupado con otras operaciones, que también afectará el rendimiento de red y la latencia. Azure resuelve este problema con redes aceleradas.

Redes aceleradas proporcionan la latencia de red ultralow coherente a través del hardware programable interna de Azure y tecnologías como SR-IOV. Redes aceleradas mueve gran parte de la Azure definido por el software de pila de red en SmartNICs basada en FPGA y las CPU. Este cambio permite que las aplicaciones del usuario final reclamar ciclos de cálculo, lo que supone una carga menor en la máquina virtual, reduce la vibración y las incoherencias en la latencia. En otras palabras, el rendimiento puede ser más determinista.

Redes aceleradas mejora el rendimiento al permitir que a la máquina virtual para omitir el host y establecer una ruta de datos directamente con un host SmartNIC invitada. Estas son algunas de las ventajas de accelerated networking:

- **Menor latencia / más paquetes por segundo (pps)**: Quitar el conmutador virtual de la ruta de datos elimina el tiempo que los paquetes que se pasan en el host para el procesamiento de directiva y aumenta el número de paquetes que se pueden procesar en la máquina virtual.

- **Inestabilidad reducida**: Procesamiento de conmutador virtual depende de la cantidad de directivas que deben aplicarse y la carga de trabajo de la CPU que se encarga del procesamiento. Descarga de la aplicación de directivas en el hardware, elimina esa variabilidad, entregar paquetes directamente a la máquina virtual, lo que elimina la comunicación de host-a-VM y todas las interrupciones de software y cambios de contexto.

- **Reducir el uso de CPU**: el pasar por alto el conmutador virtual en el host conlleva una disminución de la utilización de la CPU para procesar el tráfico de red.

Para usar las redes aceleradas, debe habilitarse explícitamente en cada máquina virtual aplicable. Consulte [crear una máquina virtual Linux con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) para obtener instrucciones.

#### <a name="receive-side-scaling"></a>Escala en lado de recepción

Recepción (RSS) de escalado es una tecnología de controlador de red que distribuye la recepción de tráfico de red de forma más eficaz mediante la distribución de procesamiento de recepción a través de varias CPU en un sistema multiprocesador. En términos sencillos, RSS permite que un sistema procesar más tráfico recibido porque usa todas las CPU disponibles en lugar de uno. Para obtener información más técnica de RSS, consulte [Introducción a escala en lado de recepción](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

Para obtener el mejor rendimiento cuando accelerated networking esté habilitado en una máquina virtual, deberá habilitar RSS. RSS también puede proporcionar ventajas en máquinas virtuales que no usan las redes aceleración. Para obtener información general de cómo determinar si RSS está habilitado y cómo habilitarlo, consulte [optimizar el rendimiento de red para Azure virtual machines](https://aka.ms/FastVM).

### <a name="tcp-timewait-and-timewait-assassination"></a>TCP TIME_WAIT y TIME_WAIT assassination

TCP TIME_WAIT es otra configuración común que afecta al rendimiento de red y la aplicación. En máquinas virtuales ocupadas que son de apertura y cierre muchos sockets, como los clientes o servidores (puerto de origen IP:Source + IP:Destination el puerto de destino), durante el funcionamiento normal del TCP, un socket determinado puede acabar en un estado TIME_WAIT durante mucho tiempo. El estado TIME_WAIT está pensado para permitir que los datos adicionales que se entregue en un socket antes de cerrarla. Por lo que las pilas de TCP/IP generalmente impedir la reutilización de un socket colocando un paquete TCP SYN del cliente de forma silenciosa.

La cantidad de tiempo que es un socket en TIME_WAIT es configurable. Puede abarcar desde 30 segundos a 240 segundos. Los sockets son un recurso finito y el número de sockets que puede usarse en un momento dado es configurable. (El número de sockets disponibles normalmente es aproximadamente 30.000). Si se consumen los sockets disponibles, o si los clientes y servidores tienen una configuración TIME_WAIT no coincidente, y una máquina virtual intenta volver a usar un socket en un estado TIME_WAIT, se producirá un error en las nuevas conexiones como TCP SYN los paquetes se descartan silenciosamente.

El valor de intervalo de puertos para sockets de salida es normalmente configurable dentro de la pila TCP/IP de un sistema operativo. Lo mismo es cierto para la configuración de TCP TIME_WAIT y reutilización del socket. Cambiar estos números potencialmente puede mejorar la escalabilidad. Sin embargo, según la situación, estos cambios podrían causar problemas de interoperabilidad. Se debe tener cuidado si cambia estos valores.

Puede usar assassination TIME_WAIT para abordar esta limitación de escala. Assassination TIME_WAIT permite un socket para reutilizarse en determinadas situaciones, como cuando el número de secuencia en el paquete IP de la nueva conexión supera el número de secuencia del último paquete de la conexión anterior. En este caso, el sistema operativo permitirá que se establezca la conexión nueva (aceptará el nuevo SYN/ACK) y Forzar cierre la conexión anterior que estaba en un estado TIME_WAIT. Esta funcionalidad se admite en máquinas virtuales Windows en Azure. Para obtener información sobre la compatibilidad con otras máquinas virtuales, póngase en contacto con el proveedor del sistema operativo.

Para obtener información acerca de cómo configurar el intervalo de puertos de origen y de configuración de TCP TIME_WAIT, consulte [configuración que puede modificarse para mejorar el rendimiento de red](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Factores de red virtual que pueden afectar al rendimiento

### <a name="vm-maximum-outbound-throughput"></a>Rendimiento máximo saliente de máquina virtual

Azure proporciona una variedad de tamaños de máquina virtual y los tipos, cada uno con una combinación diferente de funcionalidades de rendimiento. Una de estas funciones es el rendimiento de red (o ancho de banda), que se mide en megabits por segundo (Mbps). Dado que las máquinas virtuales se hospedan en hardware compartido, la capacidad de red debe compartirse equitativamente entre las máquinas virtuales con el mismo hardware. Máquinas virtuales más grandes se asignan más ancho de banda menor de las máquinas virtuales.

El ancho de banda de red asignado a cada máquina virtual se mide en el tráfico de salida de la máquina virtual. Todo el tráfico de red que deja la máquina virtual se cuenta para el límite asignado, independientemente del destino. Por ejemplo, si una máquina virtual tiene un límite de 1000 Mbps, ese límite se aplica si el tráfico saliente está destinado a otra máquina virtual en la misma red virtual o una fuera de Azure.

La entrada no se mide o no está directamente limitada. Pero hay otros factores, como los límites de almacenamiento y CPU, que pueden afectar a una máquina virtual para procesar los datos entrantes.

Redes aceleradas está diseñada para mejorar el rendimiento de red, incluida la latencia, rendimiento y uso de CPU. Redes aceleradas pueden mejorar el rendimiento de una máquina virtual, pero puede hacerlo solo hasta el ancho de banda asignado de la máquina virtual.

Máquinas virtuales de Azure tienen al menos una interfaz de red conectada a ellas. Pueden tener varios. El ancho de banda asignado a una máquina virtual es la suma de todo el tráfico saliente a través de todas las interfaces de red conectadas a la máquina. En otras palabras, se asigna el ancho de banda en una base de la máquina virtual, independientemente de cuántas interfaces de red están conectadas a la máquina.

Rendimiento de salida esperado y el número de interfaces de red admitidas cada tamaño de máquina virtual se detallan en [máquinas virtuales de tamaños para Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Para ver el rendimiento máximo, seleccione un tipo, como **de propósito General**y, a continuación, busque la sección acerca de la serie de tamaño en la página resultante (por ejemplo, "serie Dv2"). Para cada serie, hay una tabla que proporciona especificaciones de red en la última columna, que se titula "Nº máx. NIC / ancho de banda de red (Mbps) esperado."

El límite de rendimiento se aplica a la máquina virtual. El rendimiento no se ve afectado por estos factores:

- **Número de interfaces de red**: El límite de ancho de banda se aplica a la suma de todo el tráfico saliente desde la máquina virtual.

- **Las redes aceleradas**: Aunque esta característica puede resultar útil para lograr el límite publicado, no cambia el límite.

- **Destino del tráfico**: Todos los destinos se cuentan para el límite de salida.

- **Protocolo**: Todo el tráfico saliente a través de todos los protocolos cuenta para el límite.

Para obtener más información, consulte [ancho de banda de red de máquina Virtual](https://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Consideraciones de rendimiento de Internet

Como se describe en este artículo, los factores en internet y fuera del control de Azure pueden afectar al rendimiento de la red. Estos son algunos de esos factores:

- **Latencia**: El tiempo de ida y vuelta entre dos destinos puede verse afectado por problemas en redes intermedias, el tráfico que no toma la ruta de acceso de distancia "más corta" y las rutas de acceso de emparejamiento poco óptimos.

- **Pérdida de paquetes**: Pérdida de paquetes puede deberse a la congestión de la red, problemas de la ruta de acceso física y déficit de rendimiento de los dispositivos de red.

- **Tamaño y fragmentación de MTU**: Fragmentación a lo largo de la ruta de acceso puede dar lugar a retrasos en la llegada de los datos o en los paquetes que llegan desordenados, lo que pueden afectar a la entrega de paquetes.

Traceroute es una buena herramienta para medir las características de rendimiento de red (por ejemplo, pérdida de paquetes y latencia) a lo largo de cada ruta de acceso de red entre un dispositivo de origen y un dispositivo de destino.

### <a name="network-design-considerations"></a>Consideraciones de diseño de red

Junto con los aspectos descritos anteriormente en este artículo, la topología de una red virtual puede afectar al rendimiento de la red. Por ejemplo, un diseño de concentrador y radio que todo el mundo para una red virtual de concentrador de solo el tráfico de redes de direccionamiento presentará la latencia de red, lo que afectará al rendimiento general de red.

El número de dispositivos de red que atraviesa el tráfico de red también puede afectar a la latencia total. Por ejemplo, en un diseño de concentrador y radio, si el tráfico pasa a través de una aplicación virtual de red de radio y una aplicación virtual de concentrador antes del tránsito a internet, las aplicaciones virtuales de red pueden introducir latencia.

### <a name="azure-regions-virtual-networks-and-latency"></a>Regiones de Azure, las redes virtuales y latencia

Regiones de Azure se componen de varios centros de datos que existen dentro de un área geográfica general. Estos centros de datos no esté físicamente juntos. En algunos casos se separan por tanto como 10 kilómetros. La red virtual es una lógica superpuesta sobre la red de Azure del centro de datos físico. Una red virtual no implica que todas las topologías de red específica en el centro de datos.

Por ejemplo, dos máquinas virtuales que se encuentran en la misma red virtual y subred podrían estar en diferentes bastidores, filas o incluso los centros de datos. Se podría separar metros de cable de fibra óptica o kilómetros de cable de fibra óptica. Esta variación puede introducir latencia variable (diferencia de unos pocos milisegundos) entre diferentes máquinas virtuales.

La selección de ubicación geográfica de las máquinas virtuales y la potencial latencia resultante entre dos máquinas virtuales, pueden influir en la configuración de conjuntos de disponibilidad y las zonas de disponibilidad. Pero la distancia entre los centros de datos en una región es específica de la región e influido principalmente por la topología del centro de datos en la región.

### <a name="source-nat-port-exhaustion"></a>Agotamiento de puertos NAT de origen

Una implementación de Azure puede comunicarse con puntos de conexión fuera de Azure en la red pública de internet o en el espacio IP públicas. Cuando una instancia inicia una conexión de salida, Azure asigna dinámicamente la dirección IP privada a una dirección IP pública. Después de que Azure crea esta asignación, el tráfico de retorno para el flujo originado de salida puede también tener acceso a la dirección IP privada donde se originó el flujo.

Para todas las conexiones salientes, el equilibrador de carga de Azure debe mantener esta asignación para un período de tiempo. Con la naturaleza multiempresa de Azure, mantener esta asignación para cada flujo de salida para cada máquina virtual puede consumir muchos recursos. Por lo que hay límites en los que se establecen y según la configuración de la red Virtual de Azure. O bien, para decir con mayor precisión, una máquina virtual de Azure solo puede realizar un cierto número de conexiones salientes en un momento dado. Cuando se alcanzan estos límites, la máquina virtual no podrá realizar conexiones salientes más.

Pero este comportamiento es configurable. Para obtener más información acerca de SNAT y SNAT puerto agotamiento, consulte [en este artículo](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>Medición del rendimiento de red en Azure

Un número de los valores máximos de rendimiento en este artículo está relacionados con la latencia de red / ida y vuelta (RTT) de tiempo entre dos máquinas virtuales. Esta sección proporciona algunas sugerencias para la prueba de latencia/RTT y cómo probar el rendimiento de TCP y rendimiento de la red de máquina virtual. Puede ajustar y los valores de TCP/IP y de red mencionados anteriormente mediante el uso de las técnicas descritas en esta sección de prueba de rendimiento. Puede conectar los valores de tamaño de ventana, MSS, MTU y latencia en los cálculos que proporcionó anteriormente y comparar los valores máximos teóricos con valores reales que observan durante las pruebas.

### <a name="measure-round-trip-time-and-packet-loss"></a>Tiempo de ida y vuelta de medida y pérdida de paquetes

El rendimiento de TCP depende en gran medida RTT y pérdida de paquetes. La utilidad de PING disponible en Windows y Linux proporciona la manera más fácil de medir el RTT y pérdida de paquetes. La salida de PING mostrará la latencia como mínimo, máximo y promedio entre un origen y destino. También mostrará pérdida de paquetes. PING usa el protocolo ICMP de forma predeterminada. Puede usar PsPing para probar TCP RTT. Para obtener más información, consulte [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>Medir el rendimiento real de una conexión TCP

NTttcp es una herramienta para probar el rendimiento de TCP de una VM de Windows o Linux. Puede cambiar varios ajustes de TCP y, a continuación, pruebe las ventajas con NTttcp. Para obtener más información, vea estos recursos:

- [Ancho de banda y rendimiento de las pruebas (NTttcp)](https://aka.ms/TestNetworkThroughput)

- [Utilidad de NTttcp](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Medida de ancho de banda real de una máquina virtual

Puede probar el rendimiento de diferentes tipos de máquinas virtuales, accelerated networking y así sucesivamente, mediante una herramienta denominada iPerf. iPerf también está disponible en Linux y Windows. iPerf puede usar TCP o UDP para probar el rendimiento general de la red. las pruebas de rendimiento TCP de iPerf están influenciadas por los factores descritos en este artículo (como la latencia y RTT). Por lo tanto, UDP podría producir mejores resultados si desea probar el rendimiento máximo.

Para obtener más información, consulte estos artículos:

- [Solución de problemas de rendimiento de la red Expressroute](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Validación del rendimiento de la VPN en una red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Detectar comportamientos TCP ineficaces

En las capturas de paquetes, los clientes de Azure podrían ver los paquetes TCP con indicadores TCP (SACK, DUP ACK, retransmisión y retransmisión rápida) que podrían indicar problemas de rendimiento de red. Estos paquetes indican específicamente las ineficiencias de la red que se derivan de pérdida de paquetes. Pero la pérdida de paquetes no es necesariamente causado por problemas de rendimiento de Azure. Problemas de rendimiento podrían ser el resultado de problemas de aplicaciones, problemas del sistema operativo u otros problemas que podrían no estar directamente relacionada con la plataforma Azure.

Además, tenga en cuenta que algunos retransmisión y confirmaciones duplicadas son normales en una red. Protocolos TCP se han diseñado para ser confiable. Evidencia de estos paquetes TCP en una captura de paquetes no indica necesariamente un problema de red sistémico, a menos que sean excesivos.

Sin embargo, estos tipos de paquetes son indicaciones que el rendimiento TCP no logra su rendimiento máximo, por motivos que se describe en otras secciones de este artículo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido acerca de la optimización del rendimiento de TCP/IP para máquinas virtuales de Azure, desea conocer otras consideraciones para [planeamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) o [más información sobre la conexión y configuración de redes virtuales ](https://docs.microsoft.com/azure/virtual-network/).
