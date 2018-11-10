---
title: Uso de los sondeos de estado de Load Balancer para proteger un servicio | Microsoft Docs
description: Aprenda a usar los sondeos de estado para supervisar las instancias que hay detrás de Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2018
ms.author: kumud
ms.openlocfilehash: 134c7ab8156f3acb558e8b8a2da343961a6aad4e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419340"
---
# <a name="load-balancer-health-probes"></a>Sondeos de estado de Load Balancer

Azure Load Balancer usa sondeos de estado para determinar las instancias del grupo de back-end que van a recibir los nuevos flujos. Puede usar los sondeos de mantenimiento para detectar algún error de una aplicación en una instancia de back-end. También puede generar una respuesta personalizada a un sondeo de estado y usar este para el control del flujo e indicar a Load Balancer si debe continuar enviando flujos nuevos o detener el envío de flujos nuevos a una instancia de back-end. Esto se puede usar para administrar la carga o el tiempo de inactividad planeado. Cuando se genera un error en el sondeo de mantenimiento, Load Balancer deja de enviar flujos nuevos a la instancia con estado incorrecto respectiva.

Los tipos de sondeos de estado disponibles y la manera en que se comportan depende de la SKU de Load Balancer que se esté usando. Por ejemplo, el comportamiento tanto de los flujos nuevos como de los existentes depende de si un flujo es TCP o UDP, así como de la SKU de Load Balancer que se utilice.

| | SKU Estándar | SKU Básico |
| --- | --- | --- |
| [Tipos de sondeo](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Comportamiento de sondeo inactivo](#probedown) | Todos los sondeos inactivos y todos los flujos TCP continúan. | Todos los sondeos inactivos y todos los flujos TCP finalizan. | 

> [!IMPORTANT]
> Los sondeos de estado de Load Balancer parten de la dirección IP 168.63.129.16 y no deben bloquearse para que los sondeos marquen su instancia.  Para más información, consulte el apartado [Probe source IP address](#probesource) (Dirección IP de origen de sondeo).

## <a name="types"></a>Tipos de sondeo

Los sondeos de estado pueden observar cualquier puerto de una instancia de back-end, lo que incluye el puerto en el que se proporciona el servicio real. El protocolo de sondeo de estado puede configurarse para tres tipos diferentes de sondeos de estado:

- [Agentes de escucha TCP](#tcpprobe)
- [Puntos de conexión HTTP](#httpprobe)
- [Puntos de conexión HTTPS](#httpsprobe)

Los tipos de sondeos de estado disponibles varían según la SKU de Load Balancer seleccionada:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| SKU Estándar |    &#9989; |   &#9989; |   &#9989; |
| SKU Básico |   &#9989; |   &#9989; | &#10060; |

En el caso del equilibrio de carga de UDP, debe generar una señal de sondeo de estado personalizada para la instancia de back-end mediante un sondeo de estado TCP, HTTP o HTTPS.

Si se usan [reglas de equilibrio de carga de puertos de alta disponibilidad](load-balancer-ha-ports-overview.md) con [Standard Load Balancer](load-balancer-standard-overview.md), todos los puertos tienen la carga equilbrada y una sola respuesta del sondeo de estado tiene que reflejar el estado de toda la instancia.  

No se debe usar conectar mediante NAT o proxy un sondeo de estado a través de la instancia que recibe el sondeo de estado con otra instancia de la red virtual, porque ello podría provocar errores en cascada en el escenario.

Si desea probar un error de un sondeo de estado o anotar una instancia individual, puede usar un grupo de seguridad para bloquear de forma explícita el sondeo de estado (destino u [origen](#probesource)).

### <a name="tcpprobe"></a>Sondeo TCP

Los sondeos TCP inician una conexión mediante la realización de un protocolo de enlace TCP abierto de tres vías con el puerto definido.  Dicho protocolo va seguido de un protocolo de enlace TCP cerrado de cuatro vías.

El intervalo de sondeo mínimo es de 5 segundos y el número mínimo de respuestas incorrectas es 2.  La duración total no puede superar los 120 segundos.

Un sondeo TCP genera un error cuando:
* El agente de escucha TCP de la instancia no responde durante el período de tiempo de expiración.  Cuando el sondeo se anota en función del número de solicitudes de sondeo con error que se configuraron para quedarse sin respuesta antes de anotar el sondeo.
* El sondeo recibe un restablecimiento de TCP de la instancia.

#### <a name="resource-manager-template"></a>Plantilla de Resource Manager

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> Sondeo HTTP / HTTPS

> [!NOTE]
> El sondeo HTTPS solo está disponible para [Standard Load Balancer](load-balancer-standard-overview.md).

Los sondeos HTTP y HTTPS establecen una conexión TCP y emiten una solicitud HTTP GET con la ruta de acceso especificada. Ambos sondeos admiten rutas de acceso relativas para la solicitud HTTP GET. Los sondeos HTTPS son lo mismo que los sondeos de HTTP con un contenedor de Seguridad de la capa de transporte (TLS, conocida anteriormente como SSL) añadido. El sondeo de estado se marca cuando la instancia responde con un código de estado 200 de HTTP dentro del período de tiempo de espera.  De forma predeterminada estos sondeos de estado intentan comprobar el puerto de sondeo de estado configurado cada 15 segundos. El intervalo de sondeo mínimo es 5 segundos. La duración total no puede superar los 120 segundos. 

Los sondeos HTTP / HTTPS también pueden resultar útiles si desea implementar su propia lógica para quitar instancias de rotación del equilibrador de carga. Por ejemplo, podría decidir quitar una instancia si está por encima del 90 % de la CPU y devolver un estado que no es 200. 

Si usa Cloud Services y tiene roles web que utilizan w3wp.exe, también obtiene una supervisión automática de su sitio web. Los errores en el código del sitio web devuelven un estado distinto de 200 para el sondeo del equilibrador de carga.  El sondeo HTTP invalida el sondeo predeterminado del agente invitado. 

Un sondeo HTTP / HTTPS genera un error cuando:
* El punto de conexión de sondeo devuelve un código HTTP de respuesta distinto de 200 (por ejemplo, 403, 404 o 500). Esto marcará el sondeo de estado como inactivo inmediatamente. 
* El punto de conexión de sondeo no responde durante un período de tiempo de espera de 31 segundos. En función del valor del tiempo de expiración establecido, es posible que varias solicitudes de sondeo vayan sin respuesta antes de que el sondeo se marque como que no se está ejecutando (es decir, antes de que se envíen sondeos SuccessFailCount).
* El punto de conexión de sondeo cierra la conexión mediante TCP Reset.

#### <a name="resource-manager-templates"></a>Plantillas de Resource Manager

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Sondeo de agente invitado (solo clásico)

Los roles del servicio en la nube (roles de trabajo y roles web) usan un agente invitado para la supervisión del sondeo de forma predeterminada.   Esta opción se puede considerar el último recurso.  Siempre debe definir un sondeo de estado explícitamente con un sondeo TCP o HTTP. Un sondeo del agente invitado no es tan eficaz como LOS sondeos definidos explícitamente en los escenarios de la mayor parte de las aplicaciones.  

Un sondeo del agente invitado es una comprobación del agente invitado dentro de la máquina virtual. A continuación, escucha y responde con una respuesta HTTP 200 OK solo cuando la instancia está en estado Preparado. (Otros estados son Ocupado, Reciclando o Deteniendo).

Para obtener más información, consulte [Configuring the service definition file (csdef) for health probes](https://msdn.microsoft.com/library/azure/ee758710.aspx) (Configuración del archivo de definición de servicio (csdef) para los sondeos de estado) o [Get started by creating a public load balancer for cloud services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services) (Introducción a la creación de un equilibrador de carga público para servicios en la nube).

Si el agente invitado no responde con HTTP 200 OK, el equilibrador de carga marca la instancia como sin respuesta. y deja de enviar flujos a dicha instancia. El equilibrador de carga sigue comprobando la instancia. 

Si el agente invitado responde con un HTTP 200, el equilibrador de carga vuelve a enviar nuevos flujos a dicha instancia.

Cuando se usa un rol web, el código de sitio web normalmente se ejecuta en w3wp.exe, que no está supervisado por el agente invitado ni el tejido de Azure. Los errores en w3wp.exe (por ejemplo, las respuestas de HTTP 500) no se notifican al agente invitado. Por lo tanto, el equilibrador de carga no toma esa instancia fuera de la rotación.

## <a name="probehealth"></a>Sondeo de estado

Los sondeos de estado TCP, HTTP y HTTPS se consideran en buen estado y marcan la instancia del rol como en buen estado en los casos siguientes:

* El sondeo de estado es correcto la primera vez que arranca la máquina virtual.
* El número de elementos SuccessFailCount (descrito anteriormente) define el valor de los sondeos correctos que son necesarios para marcar la instancia de rol como en buen estado. Si se quitó una instancia de rol, el número de sondeos correctos y sucesivos debe ser igual o superior al valor de SuccessFailCount para marcar la instancia de rol como en ejecución.

> [!NOTE]
> Si el estado de una instancia de rol fluctúa, el equilibrador de carga espera más tiempo antes de devolver dicha instancia al estado correcto. Este tiempo de espera adicional protege el usuario y la infraestructura y es una directiva intencionada.

## <a name="probe-count-and-timeout"></a>Número de sondeos y tiempo de expiración

El comportamiento de los sondeos depende de:

* El número de sondeos correctos que permiten que una instancia se marque como activa.
* El número de sondeos con error que hacen que una instancia se marque como inactiva.

Los valores de tiempo de expiración y frecuencia establecidos en SuccessFailCount confirman si una instancia está en ejecución o no. En el portal de Azure, el tiempo de espera se establece en dos veces el valor de la frecuencia.

Una regla de equilibrio de carga tiene un sondeo de estado único que define el grupo de back-end respectivo.

## <a name="probedown"></a>Comportamiento de sondeo inactivo

### <a name="tcp-connections"></a>Conexiones TCP

Las nuevas conexiones TCP se realizará correctamente en la instancia de back-end que está en buen estado y tiene un sistema operativo invitado y una aplicación que puede aceptar un nuevo flujo.

Si se produce un error en el sondeo de estado de una instancia de back-end, las conexiones TCP establecidas con dicho back-end continúan.

Si se produce algún error en todos los sondeos de todas las instancias de un grupo de back-end, no se enviarán flujos nuevos a dicho grupo. Standard Load Balancer permitirá que los flujos de TCP establecidos continúen.  Basic Load Balancer terminará todos los flujos de TCP existente en el grupo de back-end.
 
Como el flujo siempre se establece entre el cliente y el sistema operativo invitado de la máquina virtual, un grupo con todos los sondeos inactivos provocará que un front-end no responda a los intentos de apertura de una conexión TCP, ya que no hay ninguna instancia de back-end en buen estado que reciba el flujo.

### <a name="udp-datagrams"></a>Datagramas UDP

Los datagramas UDP se entregarán a las instancias de back-end en buen estado.

UDP no tiene conexión y no hay ningún estado de flujo que realice el seguimiento para UDP. Si se produce un error en el sondeo de estado de la instancia de cualquier back-end, los flujos UDP existentes se pueden mover a otra instancia en buen estado del grupo de back-end.

Si se produce un error en todos los sondeos de todas las instancias de un grupo de back-end, los flujos UDP terminarán para las instancias de Basic Load Balancer y Standard Load Balancer.

## <a name="probesource"></a>Dirección IP de origen del sondeo

Load Balancer usa un servicio de sondeo distribuido para su modelo de mantenimiento interno. Todos los host en que residan máquinas virtuales se pueden programar para generar sondeos de estado por cada configuración de cliente. El tráfico del sondeo de estado se encuentra directamente entre el componente de la infraestructura que genera el sondeo de estado y la máquina virtual del cliente. Todos los sondeos de Load Balancer tienen como origen la dirección IP 168.63.129.16.  Al usar sus propias direcciones IP en la red virtual de Azure, se garantiza que esta dirección IP de origen del sondeo de estado es único, ya que está reservada globalmente para Microsoft.  Esta dirección es la misma en todas las regiones y no cambia. No se debe considerar un riesgo para la seguridad, porque la plataforma interna de Azure es la única que puede enviar un paquete desde esta dirección IP. 

Además de los sondeos de estado de Load Balancer, las siguientes operaciones usan esta dirección IP:

- Permite al agente de VM comunicarse con la plataforma para indicar que se encuentra en estado "Listo"
- Permite la comunicación con el servidor virtual de DNS para proporcionar resolución de nombres filtrada a los clientes que no definen servidores DNS personalizados.  Este filtro garantiza que los clientes solo pueden resolver los nombres de host de su implementación.

Para que el sondeo de estado de Load Balancer marque su instancia como activa, se **debe** permitir esta dirección IP en todos los [grupos de seguridad](../virtual-network/security-overview.md) de Azure y en las directivas de firewall locales.

Si no permite esta dirección IP en las directivas de firewall, se producirá un error en el sondeo de estado, ya que no puede acceder a su instancia.  A su vez, Load Balancer marcará la instancia como inactiva debido al error del sondeo de estado,  lo que puede provocar un error en el servicio con equilibrio de carga. 

De igual forma, no debe configurar la red virtual con el intervalo de direcciones IP que posee Microsoft que contengan 168.63.129.16.  Esto entrará en conflicto con la dirección IP del sondeo de estado.

Si tiene varias interfaces en la máquina virtual, es preciso que se asegure de que responde al sondeo en la interfaz en que lo recibió.  Esto puede requerir que se use únicamente la traducción de direcciones de red de origen para esta dirección de la máquina virtual interfaz a interfaz.

## <a name="monitoring"></a>Supervisión

[Standard Load Balancer](load-balancer-standard-overview.md), tanto público como interno, expone el estado del sondeo de mantenimiento por punto de conexión e instancia de back-end como métricas multidimensionales mediante Azure Monitor. Otros servicios de Azure o aplicaciones de terceros pueden utilizar esto. 

La instancia pública de Basic Load Balancer expone el estado del sondeo de estado resumido por grupo de back-end mediante Log Analytics.  Esta característica no está disponible para las instancias internas de Basic Load Balancer.  Puede usar [Log Analytics](load-balancer-monitor-log.md) para comprobar el estado del sondeo de estado y el número de sondeos del equilibrador de carga público. El registro se puede utilizar con Power BI o con Azure Operational Insights para proporcionar estadísticas del estado de mantenimiento del equilibrador de carga.

## <a name="limitations"></a>Limitaciones

-  Los sondeos HTTPS no admiten la autenticación mutua con un certificado de cliente.
-  SDK, PowerShell no admiten los sondeos HTTPS en la actualidad.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Load Balancer Estándar](load-balancer-standard-overview.md)
- [Empiece a crear un equilibrador de carga público en Resource Manager mediante Azure PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [API REST para los sondeos de estado](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Solicite nuevas capacidades de sondeo de estado con [Uservoice de Load Balancer](https://aka.ms/lbuservoice)
