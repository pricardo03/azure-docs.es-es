---
title: Integrar la aplicación con Azure Virtual Network - Azure App Service
description: Muestra cómo conectar una aplicación de Azure App Service a una red virtual de Azure nueva o existente
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: dcb128d8793e3438d87e728bde069d07c72cf97b
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66493007"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integración de su aplicación con una instancia de Azure Virtual Network
Este documento describe la característica de integración de la red virtual de Azure App Service y cómo configurar las aplicaciones en la [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure Virtual Network][VNETOverview] (VNET) le permiten colocar cualquier recurso de Azure en una red que se pueda enrutar distinta de Internet.  

Azure App Service adopta dos formas. 

1. Los sistemas multiinquilino que admiten la gama completa de planes de precios, excepto Aislado.
2. App Service Environment (ASE), que se implementa en la red virtual y es compatible con las aplicaciones del plan de precios aislado

Este documento se pasa a través de las dos características de integración con red virtual, que es para su uso en App Service multiinquilino. Si la aplicación está en [App Service Environment][ASEintro], ya está en una red virtual y no requiere el uso de la característica Integración con red virtual para acceder a recursos en la misma red virtual. Para obtener más información sobre todas las características de redes de App Service, lea [las características de red de App Service](networking-features.md)

Hay dos formas a la característica de integración con red virtual

1. Una versión habilita la integración con redes virtuales en la misma región. Este formulario de la característica requiere una subred de una red virtual en la misma región
2. La otra versión permite la integración con redes virtuales en otras regiones o con redes virtuales clásicas. Esta versión de la característica requiere la implementación de una puerta de enlace de red Virtual en la red virtual.

Una aplicación puede usar solo una forma de la característica de integración con red virtual a la vez. A continuación, la pregunta es qué característica debe usar. Puede usar para muchas cosas. Sin embargo son los diferenciadores claros:

| Problema  | Solución | 
|----------|----------|
| Desea tener acceso a una dirección de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) en la misma región | Integración con redes virtuales regionales |
| Póngase en contacto con una red virtual clásica o una red virtual en otra región | necesita integración con redes virtuales de puerta de enlace |
| Póngase en contacto con los puntos de conexión de RFC 1918 a través de ExpressRoute | Integración con redes virtuales regionales |
| Póngase en contacto con los recursos a través de puntos de conexión de servicio | Integración con redes virtuales regionales |

Ninguna de estas características le permitirá llegar a direcciones que no son RFC 1918 a través de ExpressRoute. Para ello, debe usar un ASE por ahora.

No conectar su red virtual en el entorno local o configurar los extremos de servicio mediante la integración de red virtual regional. Que es independiente de configuración de red. La integración con red virtual regional simplemente permite a la aplicación realizar llamadas a través de esos tipos de conexiones.

Independientemente de la versión que usa, integración con red virtual le ofrece su aplicación web acceso a los recursos de la red virtual, pero no concede acceso privado de entrada a la aplicación web desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual es únicamente para realizar llamadas salientes desde la aplicación hacia la red virtual. 

La característica Integración con red virtual:

* requiere un plan de precios Estándar, Premium o PremiumV2; 
* es compatible con TCP y UDP;
* funciona con aplicaciones de App Service y aplicaciones de función

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;

## <a name="regional-vnet-integration"></a>Integración con redes virtuales regionales 

Cuando se usa la integración con red virtual con redes virtuales en la misma región que la aplicación, requiere el uso de una subred delegado con al menos 32 direcciones en ella. La subred no se puede usar para nada más. Las llamadas salientes realizadas desde la aplicación se realizarán desde las direcciones de la subred delegada. Cuando se usa esta versión de la integración con redes virtuales, las llamadas se realizan desde direcciones de la red virtual. Uso de direcciones de la red virtual permite a la aplicación:

* realizar llamadas a servicios protegidos de punto de conexión de servicio
* acceso a los recursos a través de conexiones de ExpressRoute
* acceso a los recursos de la red virtual está conectado a
* acceso a los recursos a través de conexiones emparejadas incluidas las conexiones de ExpressRoute

Esta característica está en versión preliminar, pero se admite para cargas de trabajo de producción con las siguientes limitaciones:

* solo puede acceder a las direcciones que están en el intervalo de RFC 1918. Esos son direcciones en el 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 bloques de direcciones.
* no se puede tener acceso a recursos a través de conexiones de emparejamiento globales
* no se puede establecer rutas en el tráfico procedente de la aplicación en la red virtual
* la característica solo está disponible en unidades de escalado de App Service más recientes que admiten planes de App Service de PremiumV2.
* no se puede usar la característica de plan aislado las aplicaciones que se encuentran en un entorno de App Service
* la característica requiere una subred con al menos 32 direcciones en la VNet de Resource Manager sin usar.
* La aplicación y la red virtual deben estar en la misma región
* Se usa una dirección para cada instancia del plan de App Service. Puesto que no se puede cambiar el tamaño de la subred después de la asignación, use una subred que pueda cubrir un tamaño mayor al de su escala máxima. El tamaño /27 con 32 direcciones es el recomendado, ya podría dar cabida a un plan de App Service escalado a 20 instancias.
* No se puede eliminar una red virtual con una aplicación integrada. Debe quitar primero la integración 

Para usar la característica de integración con red virtual con una VNet de Resource Manager en la misma región:

1. Vaya a la interfaz de usuario de Redes en el portal. Si la aplicación es capaz de usar la nueva característica, verá una opción para agregar red virtual (versión preliminar).  

   ![Seleccione la integración con red virtual][6]

1. Seleccione **Agregar VNET (versión preliminar)** .  

1. Seleccione la red virtual de Resource Manager con la que quiere realizar la integración y, a continuación, cree una nueva subred o elija una subred vacía existente. La integración tarda menos de un minuto en completarse. Durante la integración, la aplicación se reinicia.  Cuando se complete la integración, verá detalles sobre la red virtual con la que se ha integrado y un banner en la parte superior, que indica que la característica está en versión preliminar.

   ![Selección de la red virtual y la subred][7]

Una vez que la aplicación se integra con la red virtual, usará el mismo servidor DNS configurado con la red virtual. 

Para desconectar la aplicación de la red virtual, seleccione **Desconectar**. Esta acción reiniciará la aplicación web. 

La nueva característica Integración con red virtual le permite usar puntos de conexión de servicio.  Para usar puntos de conexión de servicio con la aplicación, use la nueva integración con red virtual para conectarse a una red virtual seleccionada y, a continuación, configure los puntos de conexión de servicio en la subred que usó para la integración. 

### <a name="how-vnet-integration-works"></a>Funcionamiento de la integración con red virtual

Las aplicaciones en App Service se hospedan en roles de trabajo. El nivel básico o superior de planes de precios dedicados planes de hospedaje donde no hay ningún otros clientes las cargas de trabajo que se ejecutan en los trabajadores de la mismos. Integración con red virtual funciona mediante el montaje de las interfaces virtuales con direcciones de la subred delegada. Dado que el de la dirección está en la red virtual, tiene acceso a la mayoría de las cosas en o a través de la red virtual tal como haría con una máquina virtual en la red virtual. La implementación de red es diferente del que se ejecuta una máquina virtual en la red virtual y que es ¿por qué algunas características de red todavía no están disponibles al usar esta característica.

![Integración con red virtual](media/web-sites-integrate-with-vnet/vnet-integration.png)

Cuando se habilita la integración con red virtual, la aplicación todavía hará que las llamadas salientes a internet a través de los mismos canales que normal. Las direcciones de salida que se muestran en el portal de las propiedades de la aplicación siguen siendo las direcciones usadas por la aplicación. ¿Cuáles son los cambios de la aplicación que llama al punto de conexión de servicio de servicios protegidos o direcciones RFC 1918 entra en la red virtual. 

La característica solo admite una interfaz virtual por trabajo.  Una interfaz virtual por trabajo significa que una interfaz virtual por cada plan de App Service. Todas las aplicaciones en el mismo plan de App Service pueden usar el mismo integración con red virtual, pero si necesita conectarse a una red virtual adicional, deberá crear otro plan de App Service. La interfaz virtual utilizada no es un recurso que los clientes tienen acceso directo a.

Dada la naturaleza de cómo funciona esta tecnología, el tráfico que se usa con la integración con red virtual no se muestra en los registros de flujo de Network Watcher o NSG.  

## <a name="gateway-required-vnet-integration"></a>Necesita integración con redes virtuales de puerta de enlace 

La puerta de enlace requiere la característica de integración con red virtual:

* se puede usar para conectarse a redes virtuales en cualquier región sean Resource Manager o redes virtuales clásicas
* permite que una aplicación se conecte solo a una red virtual a la vez;
* permite la integración con hasta cinco redes virtuales dentro de un plan de App Service; 
* permite la misma red virtual que va a usar varias aplicaciones en un Plan de App Service sin afectar el número total que se puede utilizar un plan de App Service.  Si tiene 6 aplicaciones que usan la misma red virtual en el mismo plan de App Service, que cuenta como 1 red virtual que se va a usar. 
* requiere una puerta de enlace de red virtual que esté configurada con una VPN de punto a sitio;
* admite un SLA del 99,9 % debido al SLA de la puerta de enlace.

Esta característica no es compatible con:

* el acceso a los recursos a través de ExpressRoute; 
* el acceso a los recursos a través de puntos de conexión de servicio. 

### <a name="getting-started"></a>Introducción

Le recordamos algunos aspectos que debe tener en cuenta antes de conectar su aplicación web a una red virtual:

* Una red virtual de destino debe tener habilitada la VPN de punto a sitio con una puerta de enlace basada en rutas para que se pueda conectar a una aplicación. 
* La red virtual debe compartir la misma suscripción que el plan de App Service (ASP).
* Las aplicaciones que se integran con una red virtual usarán el DNS que se especifique para esa red virtual.

### <a name="set-up-a-gateway-in-your-vnet"></a>Configuración de una puerta de enlace en la red virtual ###

Si ya tiene una puerta de enlace configurada con direcciones de punto a sitio, puede pasar a configurar la integración con red virtual con su aplicación.  
Para crear una puerta de enlace:

1. [Cree una subred de puerta de enlace][creategatewaysubnet] en su red virtual.  

1. [Cree la instancia de VPN Gateway][creategateway]. Seleccione un tipo de VPN basada en rutas.

1. [Establezca las direcciones de punto a sitio][setp2saddresses]. Si la puerta de enlace no está en la SKU básica, IKEV2 debe estar deshabilitado en la configuración de punto a sitio y SSTP debe estar seleccionado. El espacio de direcciones debe estar en los bloques de direcciones RFC 1918, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

Si es simplemente crear la puerta de enlace para usar con la integración de red virtual de App Service, a continuación, no es necesario cargar un certificado. La creación de la puerta de enlace puede tardar 30 minutos. No podrá integrar su aplicación con la red virtual hasta que la puerta de enlace esté aprovisionada. 

### <a name="configure-vnet-integration-with-your-app"></a>Configuración de integración con red virtual con su aplicación 

Para habilitar la integración con red virtual en su aplicación: 

1. Vaya a la aplicación en Azure Portal, abra la configuración de la aplicación y seleccione Redes > Integración con red virtual. La página ASP debe ser en una SKU estándar o superior para poder utilizar cualquier característica de integración con red virtual. 
 ![UI de Integración con red virtual][1]

1. Seleccione **Agregar VNET**. 
 ![Agregar integración con red virtual][2]

1. Seleccione su red virtual. 
  ![Seleccionar la red virtual][8]
  
La aplicación se reiniciará después de este último paso.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Cómo la puerta de enlace requiere la característica de integración con red virtual funciona

La puerta de enlace requiere la característica de integración con red virtual se basa en tecnología VPN de punto a sitio. La tecnología de punto a sitio limita el acceso a la red únicamente para la máquina virtual que hospeda la aplicación. Las aplicaciones están restringidas para enviar solo tráfico a internet a través de conexiones híbridas o de la integración con red virtual. 

![Funcionamiento de la integración con red virtual][3]

## <a name="managing-vnet-integration"></a>Administrar la integración de red virtual
La capacidad de conectarse a una red virtual y desconectarse de ella se encuentra en un nivel de aplicación. Las operaciones que pueden afectar a la integración con red virtual en varias aplicaciones se encuentran en un nivel del plan de App Service. Desde la aplicación > redes > portal de la integración con red virtual, puede obtener detalles sobre la red virtual. Puede ver información similar en el nivel ASP en ASP > redes > portal de integración con redes virtuales, incluido qué aplicaciones en el plan de App Service usan una integración dada.

 ![Detalles de VNET][4]

La información disponible en la interfaz de usuario de integración de red virtual es el mismo entre la aplicación y los portales ASP.

* Nombre de red virtual: vínculos a la interfaz de usuario de la red virtual
* Ubicación: refleja la ubicación de la red virtual. La integración con una red virtual de otra ubicación puede causar problemas de latencia de la aplicación. 
* Estado del certificado: refleja si los certificados están sincronizados entre el plan de App Service y la red virtual.
* Estado de la puerta de enlace - debe usar la puerta de enlace requiere la integración con red virtual, puede ver el estado de la puerta de enlace.
* Espacio de direcciones de red virtual: muestra el espacio de direcciones IP de la red virtual. 
* Espacio de direcciones de punto a sitio: muestra el espacio de direcciones IP de punto a sitio de la red virtual. Al realizar llamadas en la red virtual al usar la característica de puerta de enlace necesaria, será la dirección de aplicación de una de estas direcciones. 
* Espacio de direcciones de sitio a sitio: puede usar VPN de sitio a sitio para conectar la red virtual a sus recursos locales o a otras redes virtuales. Aquí se muestran los intervalos IP definidos con esa conexión VPN.
* Servidores DNS: muestra los servidores DNS configurados con la red virtual.
* Direcciones IP enrutadas a la red virtual: muestra los bloques de direcciones enrutados que se usan para dirigir el tráfico hacia la red virtual. 

La única operación que puede realizar en la vista de aplicación de Integración con red virtual es desconectar la aplicación de la red virtual si está conectada a ella. Para desconectar la aplicación de una red virtual, seleccione **Desconectar**. La aplicación se reiniciará cuando se desconecte de una red virtual. La desconexión no cambia la red virtual. No se quita la subred o la puerta de enlace. Si desea eliminar la red virtual, deberá desconectar la aplicación de la red virtual en primer lugar y eliminar los recursos en ella como puertas de enlace. 

Para llegar a la interfaz de usuario de integración de red virtual del plan de App Service, abra la interfaz de usuario de dicho plan y seleccione **Redes**.  En Integración con red virtual, seleccione **Haga clic aquí para configurar** para abrir la interfaz de usuario de Estado de característica de red.

![Información de integración con red virtual del plan de App Service][5]

La interfaz de usuario de integración de red virtual del plan de App Service le mostrará todas las redes virtuales que usan las aplicaciones de su plan de App Service. Para ver más detalles sobre cada red virtual, haga clic en la red virtual que le interese. Hay dos acciones que puede realizar aquí.

* **Sincronizar red**. La operación de sincronización de red es solo para la característica de integración con red virtual dependiente de puerta de enlace. Realizar una operación de sincronización de red garantiza que los certificados y la información de red estén sincronizados. Si agrega o cambia el DNS de la red virtual, debe ejecutar la operación **Sincronizar red**. Esta operación reiniciará todas las aplicaciones con esta red virtual.
* **Agregar rutas** La adición de rutas dirigirá el tráfico saliente hacia la red virtual.

**Enrutamiento** Las rutas definidas en la red virtual se usan para dirigir el tráfico desde la aplicación hacia la red virtual. Si necesita enviar más tráfico saliente a la red virtual, puede agregar aquí los bloques de direcciones. Esta capacidad para la que sólo funciona con la puerta de enlace requiere la integración con red virtual.

**Certificados** cuando la puerta de enlace requiere la integración con red virtual habilitada, hay un intercambio obligatorio de certificados para garantizar la seguridad de la conexión. Junto con los certificados, se incluyen la configuración de DNS, las rutas y otros elementos similares que describen la red.
Si los certificados o la información de red cambian, es necesario hacer clic en "Sincronizar red". Al hacer clic en "Sincronizar red", se producirá una breve interrupción en la conectividad entre la aplicación y la red virtual. Aunque no se reinicia la aplicación, la pérdida de conectividad podría hacer que su sitio no funcione correctamente. 

## <a name="accessing-on-premises-resources"></a>Obtener acceso a recursos locales
Las aplicaciones pueden acceder a los recursos locales mediante la integración con redes virtuales que tengan conexiones de sitio a sitio. Si usa la puerta de enlace requiere la integración con red virtual, deberá actualizar las rutas de puerta de enlace VPN local con los bloques de direcciones de punto a sitio. En la configuración inicial de la VPN de sitio a sitio, los scripts que se usan para configurarla deben configurar las rutas correctamente. Si agrega las direcciones de punto a sitio después de crear la VPN de sitio a sitio, deberá actualizar las rutas manualmente. El procedimiento varía según la puerta de enlace y no se describe aquí. No puede tener BGP configurado con una conexión VPN de sitio a sitio.

No hay ninguna configuración adicional necesario para que la característica de integración con red virtual regional alcanzar a través de la red virtual y en el entorno local. Basta con conectar su red virtual en el entorno local mediante ExpressRoute o VPN de sitio a sitio. 

> [!NOTE]
> La puerta de enlace requiere la característica de integración con red virtual no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Aunque la puerta de enlace de ExpressRoute esté configurada en [modo de coexistencia][VPNERCoex], la integración con red virtual no funcionará. Si necesita tener acceso a recursos a través de una conexión ExpressRoute, puede usar la característica de integración con red virtual regional o una [App Service Environment][ASE], que se ejecuta en la red virtual. 
> 
> 

## <a name="peering"></a>Emparejamiento
Si usas el emparejamiento con la integración con red virtual regional, no es necesario realizar ninguna configuración adicional. 

Si usa la puerta de enlace requiere la integración con redes virtuales con emparejamiento, deberá configurar algunos elementos adicionales. Para configurar el emparejamiento para que funcione con su aplicación:

1. Agregue una conexión de emparejamiento en la red virtual a la que se conecta su aplicación. Al agregar la conexión de emparejamiento, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Permitir tránsito de puerta de enlace**.
1. Agregue una conexión de emparejamiento en la red virtual que se está emparejando con la red virtual a la que está conectado. Al agregar la conexión de emparejamiento en la red virtual de destino, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Allow remote gateways** (Permitir puertas de enlace remotas).
1. Vaya a Plan de App Service > Redes > UI de Integración con red virtual en el portal.  Seleccione la red virtual a la que está conectada su aplicación. En la sección de enrutamiento, agregue el intervalo de direcciones de la red virtual que está emparejada con la red virtual a la que está conectada su aplicación.  


## <a name="pricing-details"></a>Detalles de precios
La característica de integración con red virtual regional no tiene ningún cargo adicional por uso más allá de lo cargos de nivel de precios de ASP.

Hay tres cargos relacionados con el uso de la característica de integración con redes virtuales de puerta de enlace necesaria:

* Cargos de nivel de precios de ASP: las aplicaciones deben estar en un estándar, Premium o PremiumV2 Plan de App Service. Puede ver más detalles sobre esos costos aquí: [Precios de App Service][ASPricing]. 
* Costos de transferencia de datos - allí es un cargo de salida de datos, incluso si la red virtual está en el mismo centro de datos. Estos cargos se describen en la página de [detalles de precios de transferencia de datos][DataPricing]. 
* Los costos de la puerta de enlace VPN - allí es un costo para la puerta de enlace de red virtual que se requiere para la VPN de punto a sitio. Encontrará los detalles en la página [Precios de VPN Gateway][VNETPricing].


## <a name="troubleshooting"></a>solución de problemas
El que una característica sea fácil de configurar no quiere decir que no presente problemas con el uso. Si encuentra problemas para acceder al punto de conexión que desee, existen varias utilidades que sirven para probar la conectividad desde la consola de la aplicación. Dispone de dos consolas que puede usar. Una es la consola Kudu y la otra es la consola a la que se accede en Azure Portal. Para acceder a la consola Kudu desde la aplicación, vaya a Herramientas -> Kudu. Esto equivale a ir a [nombreDeSitio].scm.azurewebsites.net. Cuando se abra, vaya a la pestaña de consola Depurar. Para llegar a la consola hospedada en el Portal de Azure, desde su aplicación, vaya a Herramientas -> Consola. 

#### <a name="tools"></a>Herramientas
Las herramientas **ping**, **nslookup** y **tracert** no funcionarán a través de la consola por restricciones de seguridad. Para suplir esta falta, se han agregado dos herramientas independientes. Para probar la funcionalidad de DNS, hemos agregado una herramienta denominada nameresolver.exe. La sintaxis es:

    nameresolver.exe hostname [optional: DNS Server]

Puede usar **nameresolver** para comprobar los nombres de host de los que depende la aplicación. De este modo, puede probar si hay algo mal configurado en el DNS o si no tiene acceso al servidor DNS. Puede ver el servidor DNS que va a usar en la consola de la aplicación examinando las variables de entorno WEBSITE_DNS_SERVER y WEBSITE_DNS_ALT_SERVER.

La siguiente herramienta permite probar la conectividad de TCP para una combinación de host y puerto. Esta herramienta se llama **tcpping** y la sintaxis es:

    tcpping.exe hostname [optional: port]

La utilidad **tcpping** indica si se puede llegar a un host y puerto específicos. Solo puede mostrar correcto si hay una aplicación que escucha en la combinación de host y puerto, y no hay acceso a la red de la aplicación para el host especificado y el puerto.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Depuración del acceso a recursos hospedados en la red virtual
Hay varias situaciones que pueden impedir que la aplicación llegue a un host y un puerto específicos. La mayoría de las veces se debe a una de estas tres causas:

* **Existe un firewall que lo impide.** Si tiene un firewall que lo impide, se agotará el tiempo de espera de TCP. El tiempo de espera de TCP es de 21 segundos en este caso. Utilice la herramienta **tcpping** para probar la conectividad. Los tiempos de espera agotados de TCP pueden deberse a muchas causas, además de los firewalls, pero comience por comprobar los firewalls. 
* **El DNS no es accesible.** El tiempo de espera de DNS es de tres segundos por cada servidor DNS. Si tiene dos servidores DNS, el tiempo de espera es de seis segundos. Utilice nameresolver para ver si funciona el DNS. Recuerde que no puede usar nslookup porque este no usa el DNS con el que se ha configurado la red virtual. Si, podría tener un firewall o bloqueando el acceso a DNS o bien NSG puede estar inactiva.

Si estos elementos no resuelven los problemas, busque primero para cosas como: 

**Integración con redes virtuales regionales**
* es el destino de una dirección de RFC 1918
* ¿hay una salida bloqueo de NSG de la subred de integración
* ¿Si va a través de una VPN o ExpressRoute, es la puerta de enlace local configurado para enrutar el tráfico de copia de seguridad en Azure? Si puede alcanzar los puntos de conexión en la red virtual, pero no en el entorno local, esto es bueno comprobar.

**necesita integración con redes virtuales de puerta de enlace**
* ¿es el intervalo de direcciones de punto a sitio en los intervalos de RFC 1918 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255)?
* ¿Aparece la puerta de enlace como activa en el Portal? Si la puerta de enlace está inactiva, vuelva a activarla.
* ¿Indican los certificados que están sincronizados o sospecha que se ha cambiado la configuración de red?  Si los certificados no están sincronizados o sospecha que ha habido un cambio realizado en la configuración de red virtual que no se ha sincronizado con su ASP, a continuación, presione "Sincronizar red".
* ¿Si va a través de una VPN o ExpressRoute, es la puerta de enlace local configurado para enrutar el tráfico de copia de seguridad en Azure? Si puede alcanzar los puntos de conexión en la red virtual, pero no en el entorno local, esto es bueno comprobar.

Depuración de problemas de red es un desafío ya existe no puede ver lo que está bloqueando el acceso a una combinación de host: puerto específico. Algunas de las causas son:

* tiene un firewall en el host que impide el acceso al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio. Para cruzar subredes, se requiere a menudo acceso público.
* el host de destino está inactivo;
* la aplicación está inactiva;
* la dirección IP o el nombre de host son incorrectos;
* la aplicación escucha en un puerto diferente del que se esperaba. Puede hacer coincidir el id. de proceso con el puerto de escucha usando "netstat -aon" en el host del punto de conexión. 
* los grupos de seguridad de la red están configurados de tal manera que impiden el acceso al host y al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio.

Recuerde que no sabe qué dirección de la aplicación utilizará realmente. Podría ser cualquier dirección, en el intervalo de direcciones integración punto a sitio o la subred, por lo que necesita permitir el acceso desde el intervalo de direcciones completa. 

Otros pasos de depuración son:

* conectarse a una VM de la red virtual e intentar acceder al recurso host:puerto desde ella. Para probar el acceso de TCP, use el comando **test-netconnection** de PowerShell. La sintaxis es:

      test-netconnection hostname [optional: -Port]

* abrir una aplicación en una máquina virtual y probar el acceso a dicho host y el puerto desde la consola de la aplicación mediante **tcpping**

#### <a name="on-premises-resources"></a>Recursos locales ####

Si la aplicación no puede conectar con un recurso local, compruebe si puede acceder al recurso desde su red virtual. Use el comando **test-netconnection** de PowerShell para comprobar el acceso de TCP. Si la máquina virtual no puede llegar a su recurso local, la VPN o conexión ExpressRoute no esté configurada correctamente.

Si la máquina virtual hospedada en la red virtual puede acceder a su sistema local, pero la aplicación no, la causa es probablemente una de las razones siguientes:

* las rutas no están configuradas con la subred o seleccione intervalos de direcciones de sitio en la puerta de enlace local
* Los grupos de seguridad de red están bloqueando el acceso del intervalo IP de punto a sitio.
* Los firewalls locales están bloqueando el tráfico procedente del intervalo IP de punto a sitio.
* está intentando tener acceso a una dirección que no son RFC 1918 mediante la característica de integración con red virtual regional


## <a name="powershell-automation"></a>Automatización de PowerShell

Puede integrar App Service con Azure Virtual Network mediante PowerShell. Para obtener un script que esté listo para ejecutarse, consulte [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3) (Conectar una aplicación de Azure App Service a una red de Azure Virtual Network).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
