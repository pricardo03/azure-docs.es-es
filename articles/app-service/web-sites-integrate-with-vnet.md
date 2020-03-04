---
title: Integración de aplicaciones con Azure Virtual Network
description: Integre aplicaciones en Azure App Service con Azure Virtual Network.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 76139716fe11536faa0ff792185ba1643801c641
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649075"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integración de su aplicación con una instancia de Azure Virtual Network
En este documento, se describe la característica Integración con red virtual de Azure App Service y se explica cómo configurarla con aplicaciones en [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure Virtual Network][VNETOverview] (redes virtuales) le permite colocar cualquier recurso de Azure en una red que se pueda enrutar distinta de Internet.  

Azure App Service adopta dos variantes. 

1. Los sistemas multiinquilino que admiten la gama completa de planes de precios, excepto Aislado.
2. App Service Environment (ASE), que se implementa en la red virtual y admite aplicaciones de planes de precios aislados.

En este documento, se explica la característica Integración con red virtual, que está indicada para instancias multiinquilino de App Service. Si la aplicación está en [App Service Environment][ASEintro], ya está en una red virtual y no requiere el uso de la característica Integración con red virtual para acceder a recursos en la misma red virtual. Para más información sobre todas las características de redes de App Service, vea [Características de redes de App Service](networking-features.md).

La característica Integración con red virtual proporciona a la aplicación web acceso a los recursos de la red virtual, pero no concede acceso privado de entrada a la aplicación web desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. La característica Integración con red virtual se comporta de forma diferente cuando se usa con redes virtuales de la misma región y con redes virtuales de otras regiones. Esta característica tiene dos variantes.

1. Versión regional de Integración con red virtual: si se conecta a redes virtuales de Resource Manager de la misma región, debe tener una subred dedicada en la red virtual con la que realizar la integración. 
2. Versión de Integración con red virtual que necesita una puerta de enlace: para conectarse a redes virtuales de otras regiones o a una red virtual clásica de la misma región, necesita una puerta de enlace de Virtual Network aprovisionada en la red virtual de destino.

Integración con red virtual tiene las siguientes características:

* Necesita un plan de precios Estándar, Premium, PremiumV2 o Grupo elástico Premium. 
* Es compatible con TCP y UDP.
* Funciona con aplicaciones de App Service y de Functions.

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;

Cuando Integración con red virtual necesita una puerta de enlace, solo proporciona acceso a los recursos de la red virtual de destino o de las redes conectadas a esta que tienen emparejamiento o redes privadas virtuales. Cuando Integración con red virtual necesita una puerta de enlace, no permite acceder a los recursos disponibles en las conexiones de ExpressRoute ni funciona con puntos de conexión de servicio. 

Independientemente de la versión que se use, Integración con red virtual ofrece a su aplicación web acceso a los recursos de su red virtual, pero no concede acceso privado entrante a su aplicación web desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. 

## <a name="enable-vnet-integration"></a>Habilitación de Integración con red virtual 

1. Vaya a la interfaz de usuario de Redes en el portal de App Service. En Integración con red virtual, seleccione *"Haga clic aquí para configurar"* . 

1. Seleccione **Agregar VNET**.  

   ![Selección de Integración con red virtual][1]

1. La lista desplegable contendrá todas las redes virtuales de Resource Manager de la suscripción que están en la misma región y, a continuación, una lista de todas las redes virtuales de Resource Manager que están en las demás regiones. Seleccione la red virtual con la que desea realizar la integración.

   ![Seleccione la red virtual.][2]

   * Si la red virtual está en la misma región, cree una nueva subred o seleccione una subred vacía existente. 

   * Para seleccionar una red virtual de otra región, necesita una puerta de enlace de Virtual Network aprovisionada con la conectividad de punto a sitio habilitada.

   * Si desea realizar la integración con una red virtual clásica, en lugar de hacer clic en la lista desplegable de redes virtuales, seleccione **Haga clic aquí para conectarse a una red virtual clásica**. Seleccione la red virtual clásica que desee. Para seleccionar una red virtual de destino, debe tener una puerta de enlace de Virtual Network aprovisionada con la conectividad de punto a sitio habilitada.

    ![Seleccione una red virtual clásica.][3]
    
Durante la integración, la aplicación se reinicia.  Una vez que se complete la integración, podrá ver los detalles en la red virtual con la que ha realizado la integración. 

Cuando la aplicación esté integrada con la red virtual, usará el mismo servidor DNS que está configurado en la red virtual, a menos que pertenezca a Azure DNS Private Zones. En la actualidad, Integración con red virtual no puede utilizarse con Azure DNS Private Zones.

## <a name="regional-vnet-integration"></a>Integración con red virtual regional

Cuando se utiliza la versión regional de Integración con red virtual, la aplicación puede acceder a:

* recursos de la red virtual que están en la misma región con la que se está realizando la integración, 
* recursos de redes virtuales emparejadas con la red virtual que se encuentran en la misma región,
* servicios protegidos mediante puntos de conexión de servicio,
* recursos de diferentes conexiones de ExpressRoute,
* recursos de la red virtual a la que está conectado,
* recursos de diferentes conexiones emparejadas, incluidas conexiones de ExpressRoute,
* puntos de conexión privados. 

Si Integración con red virtual se utiliza con redes virtuales de la misma región, se pueden usar las siguientes características de Redes de Azure:

* Grupos de seguridad de red (NSG): puede bloquear el tráfico de salida con un grupo de seguridad de red situado en la subred de integración. Las reglas de entrada no se aplicarán mientras no utilice Integración con red virtual para proporcionar acceso de entrada a la aplicación web.
* Tablas de rutas (UDR): puede colocar una tabla de rutas en la subred de integración para enviar el tráfico de salida donde quiera. 

De forma predeterminada, la aplicación solo enrutará el tráfico de RFC 1918 a la red virtual. Si desea enrutar todo el tráfico de salida a la red virtual, aplique el valor WEBSITE_VNET_ROUTE_ALL a la aplicación. Para configurar este valor de la aplicación:

1. Vaya a la interfaz de usuario de configuración del portal de la aplicación. Haga clic en **Nueva configuración de la aplicación**.
1. Escriba **WEBSITE_VNET_ROUTE_ALL** en el campo Nombre y **1** en el campo Valor.

   ![Configuración del valor de la aplicación][4]

1. Seleccione **Aceptar**.
1. Seleccione **Guardar**.

Si enruta todo el tráfico de salida a la red virtual, dicho tráfico estará sujeto a los grupos de seguridad de red y las rutas definidas por los usuarios que se apliquen a la subred de integración. Si enruta todo el tráfico de salida a la red virtual, las direcciones salientes seguirán siendo las que aparecen en las propiedades de la aplicación, a menos que proporcione otras rutas para enviar allí el tráfico. 

Existen algunas limitaciones cuando se la característica Integración con red virtual se utiliza con redes virtuales que están en la misma región:

* No se puede tener acceso a los recursos en conexiones de emparejamiento global.
* La característica solo está disponible en las unidades de escalado de App Service más recientes que admiten planes PremiumV2 de App Service.
* La subred de integración solo se puede usar en un plan de App Service.
* Las aplicaciones de plan Aislado que estén en un App Service Environment no pueden usar la característica.
* La característica necesita una subred sin usar que sea /27 con un mínimo de 32 direcciones en la red virtual de Resource Manager.
* La aplicación y la red virtual deben estar en la misma región
* No puede eliminar una red virtual con una aplicación integrada. Tiene que quitar la integración antes de eliminar la red virtual. 
* Solo puede realizar la integración con redes virtuales que estén en la misma suscripción que la aplicación web.
* Solo se puede tener una característica Integración con red virtual regional por plan de App Service. Varias aplicaciones en el mismo plan de App Service pueden usar la misma red virtual. 
* No se puede cambiar la suscripción de una aplicación o un plan de App Service mientras haya una aplicación que use la integración de red virtual regional.

Se usa una dirección para cada instancia del plan de App Service. Si escala la aplicación a cinco instancias, se utilizarán cinco direcciones. Puesto que no se puede cambiar el tamaño de la subred después de la asignación, debe usar una subred lo suficientemente grande como para dar cabida a cualquier escala que pueda alcanzar la aplicación. Un tamaño de /26 con 64 direcciones es el recomendado. Una subred /26 con 64 direcciones podrá aceptar un plan Premium de App Service con 30 instancias. Al escalar o reducir verticalmente un plan de App Service, necesitará el doble de direcciones durante un breve período de tiempo. 

Si desea que las aplicaciones de otro plan de App Service lleguen a una red virtual a la que están conectadas las aplicaciones en otro plan de App Service, debe seleccionar una subred diferente de la usada por la integración de red virtual existente.  

Esta característica se encuentra en versión preliminar para Linux. La versión para Linux de esta característica solo permite realizar llamadas a direcciones RFC 1918 (10.0.0.0/8, 172.16.0.0/12 y 192.168.0.0/16).

### <a name="web-app-for-containers"></a>Web App for Containers

Si usa App Service en Linux con las imágenes integradas, la versión regional de Integración con red virtual funcionará sin necesidad de realizar más cambios. Si usa Web App for Containers, deberá modificar la imagen de Docker para poder usar Integración de red virtual. En la imagen de Docker, use la variable de entorno PORT como puerto de escucha del servidor web principal en lugar de un número de puerto codificado de forma rígida. La variable de entorno PORT la establece automáticamente la plataforma de App Service en el momento de inicio del contenedor. Si usa SSH, el demonio de SSH debe estar configurado para escuchar en el número de puerto que especificó la variable de entorno SSH_PORT al usar la integración de red virtual regional.  En Linux, no se puede utilizar la versión de Integración con red virtual que necesita una puerta de enlace. 

### <a name="service-endpoints"></a>Puntos de conexión de servicio

La versión regional de Integración con red virtual permite utilizar puntos de conexión de servicio.  Si desea usar puntos de conexión de servicio con la aplicación, utilice la versión regional de Integración con red virtual para conectarse a una red virtual seleccionada y configurar después los puntos de conexión de servicio en la subred que utilice para realizar la integración. 

### <a name="network-security-groups"></a>Grupos de seguridad de red

Los grupos de seguridad de red permiten bloquear el tráfico de entrada y salida de los recursos de una red virtual. Las aplicaciones web que utilicen la versión regional de Integración con red virtual, podrán utilizar un [grupo de seguridad de red][VNETnsg] para bloquear el tráfico de salida hacia los recursos de la red virtual o Internet. Para bloquear el tráfico dirigido a direcciones públicas, debe tener el valor WEBSITE_VNET_ROUTE_ALL de la aplicación establecido en 1. Las reglas de entrada de un grupo de seguridad de red no tendrían efecto sobre la aplicación, ya que Integración con red virtual solo afecta al tráfico que sale de la aplicación. Para controlar el trafico de entrada en la aplicación web, utilice la característica Restricciones de acceso. Un grupo de seguridad de red que se aplique a la subred de integración entrará en vigor con independencia de las rutas aplicadas a la subred de integración. Si el valor WEBSITE_VNET_ROUTE_ALL estuviera establecido en 1 y no hubiera ninguna ruta que afectara al tráfico de direcciones públicas en la subred de integración, todo el tráfico de salida seguiría estando sujeto a los grupos de seguridad de red asignados a la subred de integración. Si WEBSITE_VNET_ROUTE_ALL no estuviera definido, los grupos de seguridad de red solo se aplicarían al tráfico de RFC 1918.

### <a name="routes"></a>Rutas

Las tablas de rutas permiten enrutar el tráfico de salida de la aplicación a cualquier sitio que se desee. De forma predeterminada, las tablas de rutas solo afectarán al tráfico de destino de RFC 1918.  Si establece WEBSITE_VNET_ROUTE_ALL en 1, solo se verán afectadas las llamadas salientes. Las rutas establecidas en la subred de integración no afectarán a las respuestas de las solicitudes de entrada de la aplicación. Los destinos más habituales suelen ser puertas de enlace o dispositivos de firewall. Si desea enrutar todo el tráfico de salida del entorno local, puede utilizar una tabla de rutas para enviar el tráfico de salida a la puerta de enlace de ExpressRoute. Si no enruta el tráfico a una puerta de enlace, asegúrese de establecer las rutas en la red externa para poder enviar de vuelta las respuestas.

Las rutas del Protocolo de puerta de enlace de borde (BGP) también tendrán efecto sobre el tráfico de la aplicación. Si tiene rutas de BGP cuyo origen es algo similar a una puerta de enlace de ExpressRoute, el tráfico de salida de la aplicación se verá afectado. De forma predeterminada, las rutas de BGP solo afectan al tráfico de destino de RFC 1918. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1, todo el tráfico de salida puede verse afectado por las rutas de BGP. 

### <a name="how-regional-vnet-integration-works"></a>Funcionamiento de la versión regional de Integración con red virtual

Las aplicaciones en App Service se hospedan en roles de trabajo. Los planes de precios básico y más elevados son planes de hospedaje dedicados donde no hay ninguna otra carga de trabajo de cliente ejecutándose en los mismos trabajos. La versión regional de Integración con red virtual funciona montando interfaces virtuales con las direcciones de la subred delegada. Como la dirección de origen está en la red virtual, tiene acceso a la mayoría de las cosas que están en esa red virtual o a las que se puede acceder a través de ella, igual que cualquier máquina virtual que estuviera en la red virtual. La implementación de red no es lo mismo que ejecutar una máquina virtual en la red virtual, y ese es el motivo por el que algunas características de red todavía no están disponibles al usar esta característica.

![Funcionamiento de la versión regional de Integración con red virtual][5]

Cuando se habilita la versión regional de Integración con red virtual, la aplicación sigue realizando llamadas salientes a Internet utilizando los mismos canales, como hace habitualmente. Las direcciones salientes que se muestran en el portal de propiedades de la aplicación siguen siendo las direcciones usadas por la aplicación. Lo que es distinto para la aplicación es que las llamadas a servicios protegidos de punto de conexión de servicio o direcciones de RFC 1918 tienen lugar en la red virtual. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1, todo el tráfico de salida puede enviarse a la red virtual. 

La característica solo admite una interfaz virtual por trabajo.  Una interfaz virtual por trabajo significa una característica Integración con red virtual regional por plan de App Service. Todas las aplicaciones en el mismo plan de App Service pueden usar la misma característica Integración con red virtual, pero si necesita que una aplicación se conecte a otra red virtual, deberá crear otro plan de App Service. La interfaz virtual utilizada no es un recurso al que los clientes tengan acceso directo.

Dada la forma en que esta tecnología funciona, el tráfico que se usa con Integración con red virtual no se muestra en los registros de flujos de NSG o Network Watcher.  

## <a name="gateway-required-vnet-integration"></a>Integración con red virtual con requisito de puerta de enlace

La versión de Integración con red virtual que necesita una puerta de enlace permite conectarse a una red virtual de otra región o a una red virtual clásica. Versión de Integración con red virtual que necesita una puerta de enlace: 

* Permite que una aplicación se conecte solo a una red virtual a la vez.
* Permite la integración con hasta cinco redes virtuales dentro de un plan de App Service. 
* Permite que varias aplicaciones de un mismo plan de App Service usen la misma red virtual sin que ello afecte al número total que se puede utilizar en un plan de App Service.  Si tiene seis aplicaciones que usan la misma red virtual en el mismo plan de App Service, solo se contabilizará una sola red virtual en uso. 
* Admite un SLA del 99,9 % debido al SLA de la puerta de enlace.
* Permite que las aplicaciones utilicen el DNS con el que se ha configurado la red virtual.
* Para poder conectarse a la aplicación, necesita una puerta de enlace basada en rutas de Virtual Network que esté configurada con una VPN de punto a sitio SSTP. 

La versión de Integración con red virtual que necesita una puerta de enlace no puede utilizarse en las siguientes circunstancias:

* Con aplicaciones de Linux
* Con una red virtual conectada a ExpressRoute 
* Para acceder a recursos protegidos por puntos de conexión de servicio
* Con una puerta de enlace coexistente que admita tanto VPN de ExpressRoute como VPN de punto a sitio o de sitio a sitio

### <a name="set-up-a-gateway-in-your-vnet"></a>Configuración de una puerta de enlace en la red virtual ###

Para crear una puerta de enlace:

1. [Cree una subred de puerta de enlace][creategatewaysubnet] en su red virtual.  

1. [Cree la instancia de VPN Gateway][creategateway]. Seleccione un tipo de VPN basada en rutas.

1. [Establezca las direcciones de punto a sitio][setp2saddresses]. Si la puerta de enlace no está en la SKU básica, IKEV2 debe estar deshabilitado en la configuración de punto a sitio, mientras que SSTP debe estar seleccionado. El espacio de direcciones de punto a sitio debe especificarse en bloques de direcciones de RFC 1918 (10.0.0.0/8, 172.16.0.0/12 y 192.168.0.0/16).

Si solo está creando la puerta de enlace para su uso con Integración con red virtual de App Service, no es necesario cargar un certificado. La creación de la puerta de enlace puede tardar 30 minutos. No podrá integrar su aplicación con la red virtual hasta que la puerta de enlace esté aprovisionada. 

### <a name="how-gateway-required-vnet-integration-works"></a>Funcionamiento de la versión de Integración con red virtual que necesita una puerta de enlace

La versión de Integración con red virtual que necesita una puerta de enlace se basa en la tecnología VPN de punto a sitio. Las VPN de punto a sitio limitan el acceso de red únicamente a la máquina virtual que hospeda la aplicación. Las aplicaciones están restringidas para enviar solo tráfico a internet a través de conexiones híbridas o de la integración con red virtual. Cuando la aplicación se ha configurado con el portal para que utilice la versión de Integración con red virtual que necesita una puerta de enlace, se entabla una compleja negociación en su nombre para crear y asignar certificados en la puerta de enlace y en la aplicación. El resultado final es que los trabajos que se usan para hospedar las aplicaciones pueden conectarse directamente a la puerta de enlace de la red virtual seleccionada. 

![Funcionamiento de la versión de Integración con red virtual que necesita una puerta de enlace][6]

### <a name="accessing-on-premises-resources"></a>Obtener acceso a recursos locales

Las aplicaciones pueden acceder a los recursos locales mediante la integración con redes virtuales que tengan conexiones de sitio a sitio. Si usa la Integración con red virtual con requisito de puerta de enlace, debe actualizar las rutas de puerta de enlace de red virtual locales con los bloques de direcciones de punto a sitio. En la configuración inicial de la VPN de sitio a sitio, los scripts que se usan para configurarla deben configurar las rutas correctamente. Si agrega las direcciones de punto a sitio después de crear la VPN de sitio a sitio, deberá actualizar las rutas manualmente. El procedimiento varía según la puerta de enlace y no se describe aquí. No se puede tener BGP configurado con una conexión VPN de sitio a sitio.

No hay ninguna configuración adicional necesaria para que la característica Integración con red virtual regional acceda a la red virtual y al entorno local. Basta con conectar la red virtual al entorno local mediante ExpressRoute o una VPN de sitio a sitio. 

> [!NOTE]
> La característica Integración con red virtual con requisito de puerta de enlace no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Aunque la puerta de enlace de ExpressRoute esté configurada en [modo de coexistencia][VPNERCoex], la integración con red virtual no funcionará. Si necesita acceder a los recursos mediante una conexión de ExpressRoute, puede utilizar la característica Integración con red virtual regional o un entorno [App Service Environment][ASE] que se ejecute en la red virtual. 
> 
> 

### <a name="peering"></a>Emparejamiento

Si se usa el emparejamiento con Integración con red virtual regional, no es necesario configurar nada más. 

Si usa la Integración con red virtual con requisito de puerta de enlace con el emparejamiento, deberá configurar algunos elementos más. Para configurar el emparejamiento para que funcione con su aplicación:

1. Agregue una conexión de emparejamiento en la red virtual a la que se conecta su aplicación. Al agregar la conexión de emparejamiento, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Permitir tránsito de puerta de enlace**.
1. Agregue una conexión de emparejamiento en la red virtual que se está emparejando con la red virtual a la que está conectado. Al agregar la conexión de emparejamiento en la red virtual de destino, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Allow remote gateways** (Permitir puertas de enlace remotas).
1. Vaya a Plan de App Service > Redes > UI de Integración con red virtual en el portal.  Seleccione la red virtual a la que está conectada su aplicación. En la sección de enrutamiento, agregue el intervalo de direcciones de la red virtual que está emparejada con la red virtual a la que está conectada su aplicación.  

## <a name="managing-vnet-integration"></a>Administración de Integración con red virtual 

La conexión y la desconexión con una red virtual tiene lugar en el nivel de la aplicación. Las operaciones que pueden afectar a la integración con red virtual en varias aplicaciones se encuentran en un nivel del plan de App Service. En la aplicación > Redes > Portal de Integración con red virtual, puede obtener detalles sobre la red virtual. Puede encontrar información similar en el nivel de ASP, en el portal ASP > Redes > Integración con red virtual.

La única operación que puede realizar en la vista de aplicación de Integración con red virtual es desconectar la aplicación de la red virtual si está conectada a ella. Para desconectar la aplicación de una red virtual, seleccione **Desconectar**. La aplicación se reiniciará cuando se desconecte de una red virtual. La desconexión no cambia la red virtual. La subred o la puerta de enlace no se quitan. Si después desea eliminar la red virtual, primero debe desconectar la aplicación de la red virtual y eliminar los recursos que hay en ella, como las puertas de enlace. 

En la interfaz de usuario de Integración con red virtual de ASP, aparecerán todas las integraciones de red virtual que se utilizan en las aplicaciones de la instancia de ASP. Para ver más detalles sobre cada red virtual, haga clic en la red virtual que le interese. Hay dos acciones que puede realizar aquí en relación con la versión de Integración con red virtual que necesita una puerta de enlace.

* **Sincronizar red**. La operación de sincronización de red solo es posible con la característica Integración con red virtual con requisito de puerta de enlace. Realizar una operación de sincronización de red garantiza que los certificados y la información de red estén sincronizados. Si agrega o cambia el DNS de la red virtual, debe ejecutar la operación **Sincronizar red**. Esta operación reiniciará todas las aplicaciones con esta red virtual.
* **Agregar rutas** La adición de rutas dirigirá el tráfico saliente hacia la red virtual. 

**Enrutamiento de la versión de Integración con red virtual que necesita una puerta de enlace**: las rutas definidas en la red virtual se utilizan para dirigir el tráfico a la red virtual desde la aplicación. Si necesita enviar más tráfico saliente a la red virtual, puede agregar aquí los bloques de direcciones. Esta capacidad solo funciona con la Integración con red virtual con requisito de puerta de enlace. Las tablas de rutas no afectan del mismo modo al tráfico de la aplicación cuando se utiliza la versión de Integración con red virtual que necesita una puerta de enlace del mismo modo que cuando se utiliza la versión regional.

**Certificados en la versión de Integración con red virtual que necesita una puerta de enlace**: cuando se habilita la versión de Integración con red virtual que necesita una puerta de enlace, es preciso intercambiar los certificados para garantizar la seguridad de la conexión. Junto con los certificados, se incluyen la configuración de DNS, las rutas y otros elementos similares que describen la red.
Si los certificados o la información de red cambian, es necesario hacer clic en "Sincronizar red". Al hacer clic en "Sincronizar red", se producirá una breve interrupción en la conectividad entre la aplicación y la red virtual. Aunque no se reinicia la aplicación, la pérdida de conectividad podría hacer que su sitio no funcione correctamente. 

## <a name="pricing-details"></a>Detalles de precios
La característica Integración con red virtual regional no tiene ningún cargo extra por uso más allá de los cargos del plan de tarifa de ASP.

Hay tres cargos relacionados con el uso de la característica Integración con red virtual con requisito de puerta de enlace:

* Cargos del plan de tarifa de ASP: las aplicaciones deben estar en un plan de App Service Estándar, Premium o PremiumV2. Puede ver más detalles sobre esos costos aquí: [Precios de App Service][ASPricing] 
* Costos de transferencias de datos: se aplica un cargo de salida de datos, aunque la red virtual esté en el mismo centro de datos. Estos cargos se describen en la página de [detalles de precios de Transferencia de datos][DataPricing]. 
* Costos de puerta de enlace de red virtual: existe un costo para la puerta de enlace de red virtual necesaria para la VPN de punto a sitio. Encontrará los detalles en la página [Precios de VPN Gateway][VNETPricing].

## <a name="troubleshooting"></a>Solución de problemas
El que una característica sea fácil de configurar no quiere decir que no presente problemas con el uso. Si encuentra problemas para acceder al punto de conexión que desee, existen varias utilidades que sirven para probar la conectividad desde la consola de la aplicación. Dispone de dos consolas que puede usar. Una es la consola Kudu y la otra es la consola a la que se accede en Azure Portal. Para acceder a la consola Kudu desde la aplicación, vaya a Herramientas -> Kudu. También puede tener acceso a la consola de Kudo en [sitename].scm.azurewebsites.net. Una vez que se cargue el sitio web, vaya a la pestaña Consola de depuración. Para llegar a la consola hospedada en el Portal de Azure, desde su aplicación, vaya a Herramientas -> Consola. 

#### <a name="tools"></a>Herramientas
Las herramientas **ping**, **nslookup** y **tracert** no funcionan en la consola por restricciones de seguridad. Para suplir esta carencia, se han agregado dos herramientas diferentes. Para probar la funcionalidad de DNS, hemos agregado una herramienta denominada nameresolver.exe. La sintaxis es:

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
* ¿El destino no es una dirección RFC 1918 y el valor WEBSITE_VNET_ROUTE_ALL no está establecido en 1?
* ¿Hay un NSG bloqueando la salida de la subred de integración?
* Si es a través de una VPN o ExpressRoute, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si puede acceder a los puntos de conexión de la red virtual pero no del entorno local, compruebe las rutas.
* ¿Tiene permisos suficientes para configurar la delegación en la subred de integración? Al configurar la versión de Integración con red virtual que necesita una puerta de enlace, la subred de integración se delegará en Microsoft.Web. La interfaz de usuario de integración de la red virtual delegará la subred en Microsoft.Web automáticamente. Si la cuenta no tiene suficientes permisos de red para establecer la delegación, necesitará que alguien que pueda configurar atributos en la subred de integración delegue la subred. Para delegar manualmente la subred de integración, vaya a la interfaz de usuario de la subred de Azure Virtual Network y establezca delegación para Microsoft.Web. 

**Integración con red virtual con requisito de puerta de enlace**
* ¿Es el intervalo de direcciones de punto a sitio en los intervalos de RFC 1918 (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255)?
* ¿Aparece la puerta de enlace como activa en el Portal? Si la puerta de enlace está inactiva, vuelva a activarla.
* ¿Aparecen los certificados como sincronizados o sospecha que la configuración de red ha cambiado?  Si los certificados no están sincronizados o sospecha que se ha producido un cambio en la configuración de red virtual que no se ha sincronizado con sus planes de App Service, haga clic en "Sincronizar red".
* Si se utiliza una red privada virtual, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si puede acceder a los puntos de conexión de la red virtual pero no del entorno local, compruebe las rutas.
* ¿Está intentando utilizar una puerta de enlace coexistente que admite tanto una conexión de punto a sitio como ExpressRoute? Las puertas de enlace coexistentes no pueden utilizarse con la característica Integración con puerta de enlace. 

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


## <a name="automation"></a>Automation

La CLI es compatible con la versión regional de Integración con red virtual. Para acceder a los comandos siguientes, [instale la CLI de Azure][installCLI]. 

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.

En la versión de Integración con red virtual que necesita una puerta de enlace, puede integrar App Service con una instancia de Azure Virtual Network utilizando PowerShell. Para obtener un script que esté listo para ejecutarse, consulte [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3) (Conectar una aplicación de Azure App Service a una red de Azure Virtual Network).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-appsetting.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


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
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[VNETRouteTables]: https://docs.microsoft.com/azure/virtual-network/manage-route-table/
[installCLI]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest/
