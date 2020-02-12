---
title: Integración de aplicaciones con Azure Virtual Network
description: Obtenga información sobre cómo Azure App Service se integra con Azure Virtual Network y cómo conectar una aplicación a una red virtual.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 08/21/2019
ms.author: ccompy
ms.custom: fasttrack-edit
ms.openlocfilehash: 472fe621fc7a95317f143ef96a1d7f8b5adfe581
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77016976"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integración de su aplicación con una instancia de Azure Virtual Network
En este documento, se describe la característica Integración con red virtual de Azure App Service y se explica cómo configurarla con aplicaciones en [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure Virtual Network][VNETOverview] (redes virtuales) le permite colocar cualquier recurso de Azure en una red que se pueda enrutar distinta de Internet.  

Azure App Service adopta dos variantes. 

1. Los sistemas multiinquilino que admiten la gama completa de planes de precios, excepto Aislado.
2. App Service Environment (ASE), que se implementa en la red virtual y admite aplicaciones de planes de precios aislados.

Este documento pasa por las dos características Integración con red virtual, pensadas para su uso en App Service multiinquilino. Si la aplicación está en [App Service Environment][ASEintro], ya está en una red virtual y no requiere el uso de la característica Integración con red virtual para acceder a recursos en la misma red virtual. Para más información sobre todas las características de redes de App Service, vea [Características de redes de App Service](networking-features.md).

La característica Integración con red virtual presenta dos formas.

1. Una versión permite la integración con redes virtuales en la misma región. Esta forma de la característica requiere que haya una subred en una red virtual en la misma región. Esta característica está aún en versión preliminar, pero se admite en cargas de trabajo de producción de aplicaciones Windows con las correspondientes salvedades señaladas en cada caso.
2. La otra versión permite la integración con redes virtuales en otras regiones o con redes virtuales clásicas. Esta versión de la característica requiere la implementación de una puerta de enlace de red virtual en la red virtual. Esta es la característica basada en VPN de punto a sitio y solo es compatible con aplicaciones de Windows.

Una aplicación puede usar solo una forma de la característica Integración con red virtual a la vez. La incógnita, pues, es qué característica usar. Puede usar cada una de ellas para muchas cosas. Estas son las diferencias más patentes entre ambas:

| Problema  | Solución | 
|----------|----------|
| Desea tener acceso a una dirección de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) en la misma región. | Integración con red virtual regional |
| Quiere tener acceso a una red virtual clásica o a una red virtual en otra región. | Integración con red virtual con requisito de puerta de enlace |
| Desea tener acceso a puntos de conexión de RFC 1918 en ExpressRoute. | Integración con red virtual regional |
| Desea tener acceso a los recursos en puntos de conexión de servicio. | Integración con red virtual regional |

Ninguna de las dos características permitirá el acceso a direcciones que no sean de RFC 1918 en ExpressRoute. Para ello, deberá usar un ASE por ahora.

El uso de Integración con red virtual regional no conecta la red virtual al entorno local ni configura puntos de conexión de servicio. Esa es otra configuración de red aparte. Integración con red virtual regional simplemente permite a la aplicación realizar llamadas a través de esos tipos de conexiones.

Independientemente de la versión que se use, Integración con red virtual ofrece a su aplicación web acceso a los recursos de su red virtual, pero no concede acceso privado entrante a su aplicación web desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. 

La característica Integración con red virtual:

* requiere un plan de precios Estándar, Premium o PremiumV2; 
* es compatible con TCP y UDP;
* funciona con aplicaciones de App Service y de Functions.

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;

## <a name="regional-vnet-integration"></a>Integración con red virtual regional 

> [!NOTE]
> El emparejamiento todavía no está disponible para App Service basado en Linux.
>

Cuando Integración con red virtual se usa con redes virtuales de la misma región que la aplicación, requiere el uso de una subred delegada con al menos 32 direcciones en ella. La subred no se puede usar para nada más. Las llamadas salientes realizadas desde la aplicación se harán desde las direcciones de la subred delegada. Cuando se usa esta versión de Integración con red virtual, las llamadas se realizan desde direcciones de la red virtual. Usar direcciones de la red virtual permite a la aplicación hacer lo siguiente:

* Realizar llamadas a servicios protegidos de punto de conexión de servicio.
* Acceder a recursos mediante conexiones de ExpressRoute.
* Acceder a los recursos de la red virtual a la que está conectado.
* Acceder a recursos a través de conexiones emparejadas, incluidas conexiones de ExpressRoute.

Esta característica está en versión preliminar, pero se admite en cargas de trabajo de producción de aplicaciones Windows con las siguientes limitaciones:

* Solo se puede acceder a las direcciones que estén en el intervalo de RFC 1918; es decir, las direcciones en los bloques de direcciones 10.0.0.0/8, 172.16.0.0/12 y 192.168.0.0/16.
* No se puede tener acceso a los recursos en conexiones de emparejamiento global.
* No se pueden establecer rutas en el tráfico procedente de la aplicación a la red virtual.
* La característica solo está disponible en las unidades de escalado de App Service más recientes que admiten planes PremiumV2 de App Service. Tenga en cuenta que esto no significa que la aplicación deba ejecutarse en una SKU de PremiumV2, solo que se debe ejecutar en un plan de App Service en el que esté disponible la opción PremiumV2 (lo que implica que se trata de una unidad de escalado más reciente donde también está disponible esta característica de integración con la red virtual).
* La subred de integración solo se puede usar en un plan de App Service.
* Las aplicaciones de plan Aislado que estén en un App Service Environment no pueden usar la característica.
* La característica requiere una subred sin usar que sea un /27 con un mínimo de 32 direcciones en la red virtual de Resource Manager.
* La aplicación y la red virtual deben estar en la misma región
* No puede eliminar una red virtual con una aplicación integrada. Para ello, antes debe quitar la integración. 
* Solo se puede tener una característica Integración con red virtual regional por plan de App Service. Varias aplicaciones en el mismo plan de App Service pueden usar la misma red virtual. 
* No se puede cambiar la suscripción de una aplicación o un plan de App Service mientras haya una aplicación que use la integración de red virtual regional.

Se usa una dirección para cada instancia del plan de App Service. Si ha escalado la aplicación a cinco instancias, se usan cinco direcciones. Puesto que no se puede cambiar el tamaño de la subred después de la asignación, debe usar una subred lo suficientemente grande como para dar cabida a cualquier escala que pueda alcanzar la aplicación. Un tamaño de /26 con 64 direcciones es el recomendado. Un tamaño de /27 con 32 direcciones tendría cabida para 20 instancias de un plan Premium de App Service si no cambió el tamaño de dicho plan. Al escalar o reducir verticalmente un plan de App Service, necesitará el doble de direcciones durante un breve período de tiempo. 

Si desea que las aplicaciones de otro plan de App Service lleguen a una red virtual a la que están conectadas las aplicaciones en otro plan de App Service, debe seleccionar una subred diferente de la usada por la integración de red virtual existente.  

La característica se encuentra también en versión preliminar para Linux. Para usar la característica Integración con red virtual con una red virtual de Resource Manager en la misma región:

1. Vaya a la interfaz de usuario de Redes en el portal. Si la aplicación es capaz de usar la nueva característica, verá una opción Agregar VNET (versión preliminar).  

   ![Selección de Integración con red virtual][6]

1. Seleccione **Agregar VNET (versión preliminar)** .  

1. Seleccione la red virtual de Resource Manager con la que quiere realizar la integración y, a continuación, cree una nueva subred o elija una subred vacía existente. La integración tarda menos de un minuto en completarse. Durante la integración, la aplicación se reinicia.  Cuando se complete la integración, verá detalles sobre la red virtual con la que se ha integrado y un banner en la parte superior, que indica que la característica está en versión preliminar.

   ![Selección de la red virtual y la subred][7]

Una vez que la aplicación esté integrada con la red virtual, usará el mismo servidor DNS que el configurado para la red virtual. 

La integración de la red virtual regional requiere que la subred de integración se delegue en Microsoft.Web.  La interfaz de usuario de integración de la red virtual delegará la subred en Microsoft.Web automáticamente. Si su cuenta no tiene suficientes permisos de red para establecer la delegación, necesitará que alguien que pueda establecer atributos en la subred de integración delegue la subred. Para delegar manualmente la subred de integración, vaya a la interfaz de usuario de la subred de Azure Virtual Network y establezca delegación para Microsoft.Web.

Para desconectar la aplicación de la red virtual, seleccione **Desconectar**. Esta acción reiniciará la aplicación web. 


#### <a name="web-app-for-containers"></a>Web App for Containers

Si usa App Service en Linux con las imágenes integradas, la característica Integración con red virtual regional funciona sin más cambios. Si usa Web App for Containers, deberá modificar la imagen de Docker para poder usar Integración de red virtual. En la imagen de Docker, use la variable de entorno PORT como puerto de escucha del servidor web principal en lugar de un número de puerto codificado de forma rígida. La variable de entorno PORT la establece automáticamente la plataforma de App Service en el momento de inicio del contenedor. Si usa SSH, el demonio de SSH debe estar configurado para escuchar en el número de puerto que especificó la variable de entorno SSH_PORT al usar la integración de red virtual regional.

### <a name="service-endpoints"></a>Puntos de conexión de servicio

La nueva característica Integración con red virtual le permite usar puntos de conexión de servicio.  Para usar puntos de conexión de servicio con la aplicación, use la nueva integración con red virtual para conectarse a una red virtual seleccionada y, a continuación, configure los puntos de conexión de servicio en la subred que usó para la integración. 


### <a name="how-vnet-integration-works"></a>Funcionamiento de la integración con red virtual

Las aplicaciones en App Service se hospedan en roles de trabajo. Los planes de precios básico y más elevados son planes de hospedaje dedicados donde no hay ninguna otra carga de trabajo de cliente ejecutándose en los mismos trabajos. Integración con red virtual funciona mediante el montaje de interfaces virtuales con direcciones en la subred delegada. Como la dirección de origen está en la red virtual, tiene acceso a la mayoría de las cosas en esa red virtual o a través de ella, igual que cualquier otra máquina virtual en la red virtual. La implementación de red no es lo mismo que ejecutar una máquina virtual en la red virtual, y ese es el motivo por el que algunas características de red todavía no están disponibles al usar esta característica.

![Integración con red virtual](media/web-sites-integrate-with-vnet/vnet-integration.png)

Cuando Integración con red virtual se habilite, la aplicación seguirá realizando llamadas salientes a Internet a través de los mismos canales, como habitualmente. Las direcciones salientes que se muestran en el portal de propiedades de la aplicación siguen siendo las direcciones usadas por la aplicación. Lo que es distinto para la aplicación es que las llamadas a servicios protegidos de punto de conexión de servicio o direcciones de RFC 1918 tienen lugar en la red virtual. 

La característica solo admite una interfaz virtual por trabajo.  Una interfaz virtual por trabajo significa una característica Integración con red virtual regional por plan de App Service. Todas las aplicaciones en el mismo plan de App Service pueden usar la misma característica Integración con red virtual, pero si necesita que una aplicación se conecte a otra red virtual, deberá crear otro plan de App Service. La interfaz virtual utilizada no es un recurso al que los clientes tengan acceso directo.

Dada la forma en que esta tecnología funciona, el tráfico que se usa con Integración con red virtual no se muestra en los registros de flujos de NSG o Network Watcher.  

## <a name="gateway-required-vnet-integration"></a>Integración con red virtual con requisito de puerta de enlace 

La característica Integración con red virtual con requisito de puerta de enlace:

* Se puede usar para conectarse a redes virtuales en cualquier región, sea Resource Manager o redes virtuales clásicas.
* Permite que una aplicación se conecte solo a una red virtual a la vez.
* Permite la integración con hasta cinco redes virtuales dentro de un plan de App Service. 
* Permite que varias aplicaciones de un mismo plan de App Service usen la misma red virtual sin que ello afecte al número total que se puede utilizar en un plan de App Service.  Si tiene seis aplicaciones que usan la misma red virtual en el mismo plan de App Service, contará como una sola red virtual en uso. 
* Requiere una puerta de enlace de red virtual que esté configurada con una VPN de punto a sitio.
* Admite un SLA del 99,9 % debido al SLA de la puerta de enlace.

Esta característica no admite lo siguiente:
* Uso con aplicaciones de Linux
* El acceso a los recursos a través de ExpressRoute 
* El acceso a los recursos a través de puntos de conexión de servicio 

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

1. [Establezca las direcciones de punto a sitio][setp2saddresses]. Si la puerta de enlace no está en la SKU básica, IKEV2 debe estar deshabilitado en la configuración de punto a sitio y SSTP debe estar seleccionado. El espacio de direcciones debe especificarse en bloques de direcciones de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

Si solo está creando la puerta de enlace para su uso con Integración con red virtual de App Service, no es necesario cargar un certificado. La creación de la puerta de enlace puede tardar 30 minutos. No podrá integrar su aplicación con la red virtual hasta que la puerta de enlace esté aprovisionada. 

### <a name="configure-vnet-integration-with-your-app"></a>Configuración de integración con red virtual con su aplicación 

Para habilitar la integración con red virtual en su aplicación: 

1. Vaya a la aplicación en Azure Portal, abra la configuración de la aplicación y seleccione Redes > Integración con red virtual. El ASP debe ser en una SKU estándar o superior para poder utilizar cualquiera de las dos características Integración con red virtual. 
 ![UI de Integración con red virtual][1]

1. Seleccione **Agregar VNET**. 
 ![Agregar integración con red virtual][2]

1. Seleccione su red virtual. 
  ![Seleccionar la red virtual][8]
  
La aplicación se reiniciará después de este último paso.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Funcionamiento de la característica Integración con red virtual con requisito de puerta de enlace

La característica Integración con red virtual con requisito de puerta de enlace se basa en tecnología VPN de punto a sitio. La tecnología de punto a sitio limita el acceso a la red únicamente para la máquina virtual que hospeda la aplicación. Las aplicaciones están restringidas para enviar solo tráfico a internet a través de conexiones híbridas o de la integración con red virtual. 

![Funcionamiento de la integración con red virtual][3]

## <a name="managing-vnet-integration"></a>Administración de Integración con red virtual
La capacidad de conectarse a una red virtual y desconectarse de ella se encuentra en un nivel de aplicación. Las operaciones que pueden afectar a la integración con red virtual en varias aplicaciones se encuentran en un nivel del plan de App Service. En la aplicación > Redes > Portal de Integración con red virtual, puede obtener detalles sobre la red virtual. Puede ver información similar en el nivel de ASP en ASP > Redes > Portal de Integración con red virtual, incluido qué aplicaciones en el plan de App Service usan una integración dada.

 ![Detalles de VNET][4]

La información disponible en la interfaz de usuario de Integración con red virtual es la misma en los portales de la aplicación y el ASP.

* Nombre de red virtual: vínculos a la interfaz de usuario de la red virtual
* Ubicación: refleja la ubicación de la red virtual. La integración con una red virtual de otra ubicación puede causar problemas de latencia de la aplicación. 
* Estado del certificado: refleja si los certificados están sincronizados entre el plan de App Service y la red virtual.
* Estado de la puerta de enlace: en caso de que deba usar la Integración con red virtual con requisito de puerta de enlace, aquí puede ver el estado de la puerta de enlace.
* Espacio de direcciones de red virtual: muestra el espacio de direcciones IP de la red virtual. 
* Espacio de direcciones de punto a sitio: muestra el espacio de direcciones IP de punto a sitio de la red virtual. Al realizar llamadas en la red virtual mientras se usa la Integración con red virtual con requisito de puerta de enlace, la dirección de origen de la aplicación será una de estas direcciones. 
* Espacio de direcciones de sitio a sitio: puede usar VPN de sitio a sitio para conectar la red virtual a sus recursos locales o a otras redes virtuales. Aquí se muestran los intervalos IP definidos con esa conexión VPN.
* Servidores DNS: muestra los servidores DNS configurados con la red virtual.
* Direcciones IP enrutadas a la red virtual: muestra los bloques de direcciones enrutados que se usan para dirigir el tráfico hacia la red virtual. 

La única operación que puede realizar en la vista de aplicación de Integración con red virtual es desconectar la aplicación de la red virtual si está conectada a ella. Para desconectar la aplicación de una red virtual, seleccione **Desconectar**. La aplicación se reiniciará cuando se desconecte de una red virtual. La desconexión no cambia la red virtual. La subred o la puerta de enlace no se quitan. Si después desea eliminar la red virtual, primero debe desconectar la aplicación de la red virtual y eliminar los recursos que hay en ella, como las puertas de enlace. 

Para llegar a la interfaz de usuario de integración de red virtual del plan de App Service, abra la interfaz de usuario de dicho plan y seleccione **Redes**.  En Integración con red virtual, seleccione **Haga clic aquí para configurar** para abrir la interfaz de usuario de Estado de característica de red.

![Información de integración con red virtual del plan de App Service][5]

La interfaz de usuario de integración de red virtual del plan de App Service le mostrará todas las redes virtuales que usan las aplicaciones de su plan de App Service. Para ver más detalles sobre cada red virtual, haga clic en la red virtual que le interese. Hay dos acciones que puede realizar aquí.

* **Sincronizar red**. La operación de sincronización de red solo es posible con la característica Integración con red virtual con requisito de puerta de enlace. Realizar una operación de sincronización de red garantiza que los certificados y la información de red estén sincronizados. Si agrega o cambia el DNS de la red virtual, debe ejecutar la operación **Sincronizar red**. Esta operación reiniciará todas las aplicaciones con esta red virtual.
* **Agregar rutas** La adición de rutas dirigirá el tráfico saliente hacia la red virtual.

**Enrutamiento** Las rutas definidas en la red virtual se usan para dirigir el tráfico desde la aplicación hacia la red virtual. Si necesita enviar más tráfico saliente a la red virtual, puede agregar aquí los bloques de direcciones. Esta capacidad solo funciona con la Integración con red virtual con requisito de puerta de enlace.

**Certificados** Cuando la Integración con red virtual con requisito de puerta de enlace está habilitada, existe un intercambio necesario de certificados para garantizar la seguridad de la conexión. Junto con los certificados, se incluyen la configuración de DNS, las rutas y otros elementos similares que describen la red.
Si los certificados o la información de red cambian, es necesario hacer clic en "Sincronizar red". Al hacer clic en "Sincronizar red", se producirá una breve interrupción en la conectividad entre la aplicación y la red virtual. Aunque no se reinicia la aplicación, la pérdida de conectividad podría hacer que su sitio no funcione correctamente. 

## <a name="accessing-on-premises-resources"></a>Obtener acceso a recursos locales
Las aplicaciones pueden acceder a los recursos locales mediante la integración con redes virtuales que tengan conexiones de sitio a sitio. Si usa la Integración con red virtual con requisito de puerta de enlace, debe actualizar las rutas de puerta de enlace de red virtual locales con los bloques de direcciones de punto a sitio. En la configuración inicial de la VPN de sitio a sitio, los scripts que se usan para configurarla deben configurar las rutas correctamente. Si agrega las direcciones de punto a sitio después de crear la VPN de sitio a sitio, deberá actualizar las rutas manualmente. El procedimiento varía según la puerta de enlace y no se describe aquí. No se puede tener BGP configurado con una conexión VPN de sitio a sitio.

No hay ninguna configuración adicional necesaria para que la característica Integración con red virtual regional acceda a la red virtual y al entorno local. Basta con conectar la red virtual al entorno local mediante ExpressRoute o una VPN de sitio a sitio. 

> [!NOTE]
> La característica Integración con red virtual con requisito de puerta de enlace no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Aunque la puerta de enlace de ExpressRoute esté configurada en [modo de coexistencia][VPNERCoex], la integración con red virtual no funcionará. Si necesita acceder a los recursos mediante una conexión de ExpressRoute, puede utilizar la característica Integración con red virtual regional o un entorno [App Service Environment][ASE] que se ejecute en la red virtual. 
> 
> 

## <a name="peering"></a>Emparejamiento
Si se usa el emparejamiento con Integración con red virtual regional, no es necesario configurar nada más. 

Si usa la Integración con red virtual con requisito de puerta de enlace con el emparejamiento, deberá configurar algunos elementos más. Para configurar el emparejamiento para que funcione con su aplicación:

1. Agregue una conexión de emparejamiento en la red virtual a la que se conecta su aplicación. Al agregar la conexión de emparejamiento, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Permitir tránsito de puerta de enlace**.
1. Agregue una conexión de emparejamiento en la red virtual que se está emparejando con la red virtual a la que está conectado. Al agregar la conexión de emparejamiento en la red virtual de destino, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Allow remote gateways** (Permitir puertas de enlace remotas).
1. Vaya a Plan de App Service > Redes > UI de Integración con red virtual en el portal.  Seleccione la red virtual a la que está conectada su aplicación. En la sección de enrutamiento, agregue el intervalo de direcciones de la red virtual que está emparejada con la red virtual a la que está conectada su aplicación.  


## <a name="pricing-details"></a>Detalles de precios
La característica Integración con red virtual regional no tiene ningún cargo extra por uso más allá de los cargos del plan de tarifa de ASP.

Hay tres cargos relacionados con el uso de la característica Integración con red virtual con requisito de puerta de enlace:

* Cargos del plan de tarifa de ASP: las aplicaciones deben estar en un plan de App Service Estándar, Premium o PremiumV2. Puede ver más detalles sobre esos costos aquí: [Precios de App Service][ASPricing] 
* Costos de transferencias de datos: se aplica un cargo de salida de datos, aunque la red virtual esté en el mismo centro de datos. Estos cargos se describen en la página de [detalles de precios de Transferencia de datos][DataPricing]. 
* Costos de puerta de enlace de red virtual: existe un costo para la puerta de enlace de red virtual necesaria para la VPN de punto a sitio. Encontrará los detalles en la página [Precios de VPN Gateway][VNETPricing].


## <a name="troubleshooting"></a>Solución de problemas
El que una característica sea fácil de configurar no quiere decir que no presente problemas con el uso. Si encuentra problemas para acceder al punto de conexión que desee, existen varias utilidades que sirven para probar la conectividad desde la consola de la aplicación. Dispone de dos consolas que puede usar. Una es la consola Kudu y la otra es la consola a la que se accede en Azure Portal. Para acceder a la consola Kudu desde la aplicación, vaya a Herramientas -> Kudu. También puede tener acceso a la consola de Kudo en [sitename].scm.azurewebsites.net. Una vez que se cargue el sitio web, vaya a la pestaña Consola de depuración. Para llegar a la consola hospedada en el Portal de Azure, desde su aplicación, vaya a Herramientas -> Consola. 

#### <a name="tools"></a>Herramientas
Las herramientas **ping**, **nslookup** y **tracert** no funcionarán a través de la consola por restricciones de seguridad. Para suplir esta falta, se han agregado dos herramientas independientes. Para probar la funcionalidad de DNS, hemos agregado una herramienta denominada nameresolver.exe. La sintaxis es:

    nameresolver.exe hostname [optional: DNS Server]

Puede usar **nameresolver** para comprobar los nombres de host de los que depende la aplicación. De este modo, puede probar si hay algo mal configurado en el DNS o si no tiene acceso al servidor DNS. Para ver el servidor DNS que la aplicación va a usar en la consola, examine las variables de entorno WEBSITE_DNS_SERVER y WEBSITE_DNS_ALT_SERVER.

La siguiente herramienta permite probar la conectividad de TCP para una combinación de host y puerto. Esta herramienta se llama **tcpping** y la sintaxis es:

    tcpping.exe hostname [optional: port]

La utilidad **tcpping** indica si se puede llegar a un host y puerto específicos. Solo puede mostrar correcto si hay una aplicación que escucha en la combinación de host y puerto, y no hay acceso a la red de la aplicación para el host especificado y el puerto.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Depuración del acceso a recursos hospedados en la red virtual
Hay varias situaciones que pueden impedir que la aplicación llegue a un host y un puerto específicos. La mayoría de las veces se debe a una de estas tres causas:

* **Existe un firewall que lo impide.** Si tiene un firewall que lo impide, se agotará el tiempo de espera de TCP. El tiempo de espera de TCP es de 21 segundos en este caso. Utilice la herramienta **tcpping** para probar la conectividad. Los tiempos de espera agotados de TCP pueden deberse a muchas causas, además de los firewalls, pero comience por comprobar los firewalls. 
* **El DNS no es accesible.** El tiempo de espera de DNS es de tres segundos por cada servidor DNS. Si tiene dos servidores DNS, el tiempo de espera es de seis segundos. Utilice nameresolver para ver si funciona el DNS. Recuerde que no puede usar nslookup porque este no usa el DNS con el que se ha configurado la red virtual. Si no puede acceder, podría haber un firewall o NSG bloqueando el acceso a DNS, o este puede estar inactivo.

Si estos elementos no resuelven el problema, plantéese cuestiones como las siguientes: 

**Integración con red virtual regional**
* ¿Es el destino de una dirección de RFC 1918?
* ¿Hay un NSG bloqueando la salida de la subred de integración?
* Si es a través de una VPN o ExpressRoute, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si se puede acceder a los puntos de conexión en la red virtual, pero no en el entorno local, conviene comprobar esto.

**Integración con red virtual con requisito de puerta de enlace**
* ¿Es el intervalo de direcciones de punto a sitio en los intervalos de RFC 1918 (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255)?
* ¿Aparece la puerta de enlace como activa en el Portal? Si la puerta de enlace está inactiva, vuelva a activarla.
* ¿Aparecen los certificados como sincronizados o sospecha que la configuración de red ha cambiado?  Si los certificados no están sincronizados o sospecha que se ha producido un cambio en la configuración de red virtual que no se ha sincronizado con sus planes de App Service, haga clic en "Sincronizar red".
* Si es a través de una VPN o ExpressRoute, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si se puede acceder a los puntos de conexión en la red virtual, pero no en el entorno local, conviene comprobar esto.

Depurar problemas de red es todo un reto porque no se puede ver lo que está bloqueando el acceso en una combinación de host y puerto específica. Algunas de las causas son:

* tiene un firewall en el host que impide el acceso al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio. Para cruzar subredes, se requiere a menudo acceso público.
* el host de destino está inactivo;
* la aplicación está inactiva;
* la dirección IP o el nombre de host son incorrectos;
* la aplicación escucha en un puerto diferente del que se esperaba. Puede hacer coincidir el id. de proceso con el puerto de escucha usando "netstat -aon" en el host del punto de conexión. 
* los grupos de seguridad de la red están configurados de tal manera que impiden el acceso al host y al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio.

Recuerde que no sabe qué dirección va a usar la aplicación realmente. Podría ser cualquier dirección en el intervalo de direcciones de punto a sitio o de subred de integración, por lo que será necesario permitir el acceso desde el intervalo de direcciones completo. 

Otros pasos de depuración son:

* conectarse a una VM de la red virtual e intentar acceder al recurso host:puerto desde ella. Para probar el acceso de TCP, use el comando **test-netconnection** de PowerShell. La sintaxis es:

      test-netconnection hostname [optional: -Port]

* Abrir una aplicación en una VM y probar el acceso a ese host y ese puerto desde la consola de la aplicación usando **tcpping**.

#### <a name="on-premises-resources"></a>Recursos locales ####

Si la aplicación no puede conectar con un recurso local, compruebe si puede acceder al recurso desde su red virtual. Use el comando **test-netconnection** de PowerShell para comprobar el acceso de TCP. Si la máquina virtual no puede acceder al recurso local, puede que la conexión de ExpressRoute o red virtual no esté configurada correctamente.

Si la máquina virtual hospedada en la red virtual puede acceder a su sistema local, pero la aplicación no, la causa es probablemente una de las razones siguientes:

* Las rutas no están configuradas con los intervalos de direcciones de punto a sitio o subred en la puerta de enlace local.
* Los grupos de seguridad de red están bloqueando el acceso del intervalo IP de punto a sitio.
* Los firewalls locales están bloqueando el tráfico procedente del intervalo IP de punto a sitio.
* Está intentando tener acceso a una dirección que no es de RFC 1918 mediante la característica Integración con red virtual regional.


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
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
