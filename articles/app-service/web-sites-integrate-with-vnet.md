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
ms.date: 11/12/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: ed99bd3626bb44bff68e4122d6b50523f19e1797
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112626"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integración de su aplicación con una instancia de Azure Virtual Network
En este documento, se describe la característica Integración con redes virtuales de Azure App Service y se muestra cómo configurarla con aplicaciones en el [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure Virtual Network][VNETOverview] (VNET) le permiten colocar cualquier recurso de Azure en una red que se pueda enrutar distinta de Internet. Después, estas redes se pueden conectar a sus redes locales mediante tecnologías de VPN. 

Azure App Service adopta dos formas. 

1. Los sistemas multiinquilino que admiten la gama completa de planes de precios, excepto Aislado.
2. La característica Entorno de App Service (ASE), que se implementa en su red virtual. 

Este documento pasa por la característica Integración con red virtual, que está pensada para su uso en la instancia App Service multiinquilino.  Si la aplicación está en [App Service Environment][ASEintro], ya está en una red virtual y no requiere el uso de la característica Integración con red virtual para acceder a recursos en la misma red virtual.

Integración con red virtual ofrece a su aplicación web acceso a los recursos de su red virtual, pero no concede acceso privado a su aplicación web desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. El acceso privado al sitio solo está disponible con un ASE configurado con un Equilibrador de carga interno (ILB). Para obtener más información sobre el uso de un ASE de ILB, comience por leer el artículo aquí indicado: [Creación y uso de un ASE de ILB][ILBASE]. 

La integración con red virtual se usa a menudo para habilitar el acceso de aplicaciones a bases de datos y servicios web que se ejecutan en su red virtual. Con Integración con red virtual, no es necesario exponer un punto de conexión público para las aplicaciones en su máquina virtual, sino que puede usar las direcciones privadas no enrutables sin conexión a Internet en su lugar. 

La característica Integración con red virtual:

* requiere un plan de precios Estándar, Premium o PremiumV2; 
* funciona con una red virtual clásica o de Resource Manager; 
* es compatible con TCP y UDP;
* funciona con aplicaciones web, móviles y de API, e instancias de Function App;
* permite que una aplicación se conecte solo a una red virtual a la vez;
* permite la integración con hasta cinco redes virtuales dentro de un plan de App Service; 
* permite que varias aplicaciones de un plan de App Service usen la misma red virtual;
* requiere una puerta de enlace de red virtual que esté configurada con una VPN de punto a sitio;
* admite un SLA del 99,9 % debido al SLA de la puerta de enlace.

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;
* el acceso privado a sitios.
* el acceso a los recursos a través de ExpressRoute; 
* el acceso a los recursos a través de puntos de conexión de servicio. 

### <a name="getting-started"></a>Introducción
Le recordamos algunos aspectos que debe tener en cuenta antes de conectar su aplicación web a una red virtual:

* Una red virtual de destino debe tener habilitada la VPN de punto a sitio con una puerta de enlace basada en rutas para que se pueda conectar a una aplicación. 
* La red virtual debe compartir la misma suscripción que el plan de App Service (ASP).
* Las aplicaciones que se integran con una red virtual usarán el DNS que se especifique para esa red virtual.

## <a name="enabling-vnet-integration"></a>Habilitación de Integración con red virtual

Hay una nueva versión de la característica Integración con red virtual que está en versión preliminar. No depende de la VPN de punto a sitio y admite el acceso a los recursos a través de ExpressRoute o de los puntos de conexión de servicio. Para obtener más información sobre la nueva funcionalidad de vista previa, vaya al final de este documento. 

### <a name="set-up-a-gateway-in-your-vnet"></a>Configuración de una puerta de enlace en la red virtual ###

Si ya tiene una puerta de enlace configurada con direcciones de punto a sitio, puede pasar a configurar la integración con red virtual con su aplicación.  
Para crear una puerta de enlace:

1. [Cree una subred de puerta de enlace][creategatewaysubnet] en su red virtual.  

1. [Cree la instancia de VPN Gateway][creategateway]. Seleccione un tipo de VPN basada en rutas.

1. [Establezca las direcciones de punto a sitio][setp2saddresses]. Si la puerta de enlace no está en la SKU básica, IKEV2 debe estar deshabilitado en la configuración de punto a sitio y SSTP debe estar seleccionado. El espacio de direcciones debe estar en uno de los siguientes bloques de direcciones:

* 10.0.0.0/8: hace referencia a un intervalo de direcciones IP de 10.0.0.0 a 10.255.255.255
* 172.16.0.0/12: hace referencia a un intervalo de direcciones IP de 172.16.0.0 a 172.31.255.255 
* 192.168.0.0/16: hace referencia a un intervalo de direcciones IP de 192.168.0.0 a 192.168.255.255

Si es simplemente crear la puerta de enlace para usar con la integración de red virtual de App Service, a continuación, no es necesario cargar un certificado. La creación de la puerta de enlace puede tardar 30 minutos. No podrá integrar su aplicación con la red virtual hasta que la puerta de enlace esté aprovisionada. 

### <a name="configure-vnet-integration-with-your-app"></a>Configuración de integración con red virtual con su aplicación ###

Para habilitar la integración con red virtual en su aplicación: 

1. Vaya a la aplicación en Azure Portal, abra la configuración de la aplicación y seleccione Redes > Integración con red virtual. Escale su plan de App Service a una SKU estándar o superior para poder configurar la integración con red virtual. 
 ![UI de Integración con red virtual][1]

1. Seleccione **Agregar VNET**. 
 ![Agregar integración con red virtual][2]

1. Seleccione su red virtual. 
  ![Seleccionar la red virtual][8]
  
La aplicación se reiniciará después de este último paso.  

## <a name="how-the-system-works"></a>Funcionamiento del sistema
La característica Integración con red virtual se basa en tecnología VPN de punto a sitio. Las aplicaciones de Azure App Service se hospedan en un sistema multiinquilino que impide el aprovisionamiento de una aplicación directamente en una red virtual. La tecnología de punto a sitio limita el acceso a la red únicamente para la máquina virtual que hospeda la aplicación. Las aplicaciones están restringidas para enviar solo tráfico a internet a través de conexiones híbridas o de la integración con red virtual. 

![Funcionamiento de la integración con red virtual][3]

## <a name="managing-the-vnet-integrations"></a>Administración de las integraciones con redes virtuales
La capacidad de conectarse a una red virtual y desconectarse de ella se encuentra en un nivel de aplicación. Las operaciones que pueden afectar a la integración con red virtual en varias aplicaciones se encuentran en un nivel del plan de App Service. Puede obtener detalles sobre la red virtual del UID de la aplicación. La misma información también se muestra en el nivel de plan de App Service. 

 ![Detalles de VNET][4]

En la página Estado de característica de red, puede ver si la aplicación está conectada a la red virtual. Si la puerta de enlace de red virtual no está disponible por cualquier razón, su estado se mostrará como no conectado. 

La información que tiene ahora disponible en la interfaz de usuario de Integración con red virtual en el nivel de aplicación es igual que la información detallada que se obtiene desde el plan de App Service. Esto es lo que cubre:

* Nombre de red virtual: vínculos a la interfaz de usuario de la red virtual
* Ubicación: refleja la ubicación de la red virtual. La integración con una red virtual de otra ubicación puede causar problemas de latencia de la aplicación. 
* Estado del certificado: refleja si los certificados están sincronizados entre el plan de App Service y la red virtual.
* Estado de la puerta de enlace: si las puertas de enlace estuvieran inactivas por algún motivo, la aplicación no podrá acceder a los recursos de la red virtual. 
* Espacio de direcciones de red virtual: muestra el espacio de direcciones IP de la red virtual. 
* Espacio de direcciones de punto a sitio: muestra el espacio de direcciones IP de punto a sitio de la red virtual. Al realizar llamadas en la red virtual, la dirección De de su aplicación será una de estas. 
* Espacio de direcciones de sitio a sitio: puede usar VPN de sitio a sitio para conectar la red virtual a sus recursos locales o a otras redes virtuales. Aquí se muestran los intervalos IP definidos con esa conexión VPN.
* Servidores DNS: muestra los servidores DNS configurados con la red virtual.
* Direcciones IP enrutadas a la red virtual: muestra los bloques de direcciones enrutados que se usan para dirigir el tráfico hacia la red virtual. 

La única operación que puede realizar en la vista de aplicación de Integración con red virtual es desconectar la aplicación de la red virtual si está conectada a ella. Para desconectar la aplicación de una red virtual, seleccione **Desconectar**. La aplicación se reiniciará cuando se desconecte de una red virtual. La desconexión no cambia la red virtual. La red virtual y su configuración, incluidas las puertas de enlace, permanecen intactas. Si después desea eliminar la red virtual, debe eliminar primero los recursos que hay en ella, incluidas las puertas de enlace. 

Para llegar a la interfaz de usuario de integración de red virtual del plan de App Service, abra la interfaz de usuario de dicho plan y seleccione **Redes**.  En Integración con red virtual, seleccione **Haga clic aquí para configurar** para abrir la interfaz de usuario de Estado de característica de red.

![Información de integración con red virtual del plan de App Service][5]

La interfaz de usuario de integración de red virtual del plan de App Service le mostrará todas las redes virtuales que usan las aplicaciones de su plan de App Service. Puede tener hasta 5 redes virtuales conectadas con cualquier número de aplicaciones en su plan de App Service. Cada aplicación puede tener una sola integración configurada. Para ver más detalles sobre cada red virtual, haga clic en la red virtual que le interese. Hay dos acciones que puede realizar aquí.

* **Sincronizar red**. La operación de sincronización de red garantiza que los certificados y la información de red estén sincronizados. Si agrega o cambia el DNS de la red virtual, debe ejecutar la operación **Sincronizar red**. Esta operación reiniciará todas las aplicaciones con esta red virtual.
* **Agregar rutas** La adición de rutas dirigirá el tráfico saliente hacia la red virtual.

**Enrutamiento** Las rutas definidas en la red virtual se usan para dirigir el tráfico desde la aplicación hacia la red virtual. Si necesita enviar más tráfico saliente a la red virtual, puede agregar aquí los bloques de direcciones.   

**Certificados** Cuando la integración con red virtual está habilitada, existe un intercambio necesario de certificados para garantizar la seguridad de la conexión. Junto con los certificados, se incluyen la configuración de DNS, las rutas y otros elementos similares que describen la red.
Si los certificados o la información de red cambian, es necesario hacer clic en "Sincronizar red". Al hacer clic en "Sincronizar red", se producirá una breve interrupción en la conectividad entre la aplicación y la red virtual. Aunque no se reinicia la aplicación, la pérdida de conectividad podría hacer que su sitio no funcione correctamente. 

## <a name="accessing-on-premises-resources"></a>Obtener acceso a recursos locales
Las aplicaciones pueden acceder a los recursos locales mediante la integración con redes virtuales que tengan conexiones de sitio a sitio. Para obtener acceso a los recursos locales, debe actualizar las rutas de VPN Gateway locales con los bloques de direcciones de punto a sitio. En la configuración inicial de la VPN de sitio a sitio, los scripts que se usan para configurarla deben configurar las rutas correctamente. Si agrega las direcciones de punto a sitio después de crear la VPN de sitio a sitio, deberá actualizar las rutas manualmente. El procedimiento varía según la puerta de enlace y no se describe aquí. Además, no puede tener BGP configurado con una conexión VPN de sitio a sitio.

> [!NOTE]
> La característica Integración con red virtual no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Aunque la puerta de enlace de ExpressRoute esté configurada en [modo de coexistencia][VPNERCoex], la integración con red virtual no funcionará. Si necesita tener acceso a los recursos mediante una conexión de ExpressRoute, puede utilizar una instancia de [App Service Environment][ASE] que se ejecute en la red virtual. 
> 
> 

## <a name="peering"></a>Emparejamiento
Puede usar la integración con red virtual para acceder a los recursos de redes virtuales que se han emparejado con la red virtual a la que está conectado. Para configurar el emparejamiento para que funcione con su aplicación:

1. Agregue una conexión de emparejamiento en la red virtual a la que se conecta su aplicación. Al agregar la conexión de emparejamiento, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Permitir tránsito de puerta de enlace**.
1. Agregue una conexión de emparejamiento en la red virtual que se está emparejando con la red virtual a la que está conectado. Al agregar la conexión de emparejamiento en la red virtual de destino, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Allow remote gateways** (Permitir puertas de enlace remotas).
1. Vaya a Plan de App Service > Redes > UI de Integración con red virtual en el portal.  Seleccione la red virtual a la que está conectada su aplicación. En la sección de enrutamiento, agregue el intervalo de direcciones de la red virtual que está emparejada con la red virtual a la que está conectada su aplicación.  


## <a name="pricing-details"></a>Detalles de precios
Hay tres cargos relacionados con el uso de la característica Integración con red virtual:

* Requisitos del plan de tarifa para el plan de servicio de aplicaciones
* Costos de la transferencia de datos
* Costos de VPN Gateway

Las aplicaciones deben estar en un plan de App Service Estándar, Premium o PremiumV2. Puede ver más detalles sobre esos costos aquí: [Precios de App Service][ASPricing]. 

Se aplica un cargo de salida de datos, aunque la red virtual esté en el mismo centro de datos. Estos cargos se describen en la página de [detalles de precios de transferencia de datos][DataPricing]. 

Existe un costo para la puerta de enlace de red virtual necesario para la VPN de punto a sitio. Encontrará los detalles en la página [Precios de VPN Gateway][VNETPricing].

## <a name="troubleshooting"></a>solución de problemas
El que una característica sea fácil de configurar no quiere decir que no presente problemas con el uso. Si encuentra problemas para acceder al punto de conexión que desee, existen varias utilidades que sirven para probar la conectividad desde la consola de la aplicación. Dispone de dos consolas que puede usar. Una es la consola Kudu y la otra es la consola a la que se accede en Azure Portal. Para acceder a la consola Kudu desde la aplicación, vaya a Herramientas -> Kudu. Esto equivale a ir a [nombreDeSitio].scm.azurewebsites.net. Cuando se abra, vaya a la pestaña de consola Depurar. Para llegar a la consola hospedada en el Portal de Azure, desde su aplicación, vaya a Herramientas -> Consola. 

#### <a name="tools"></a>Herramientas
Las herramientas **ping**, **nslookup** y **tracert** no funcionarán a través de la consola por restricciones de seguridad. Para suplir esta falta, se han agregado dos herramientas independientes. Para probar la funcionalidad de DNS, hemos agregado una herramienta denominada nameresolver.exe. La sintaxis es:

    nameresolver.exe hostname [optional: DNS Server]

Puede usar **nameresolver** para comprobar los nombres de host de los que depende la aplicación. De este modo, puede probar si hay algo mal configurado en el DNS o si no tiene acceso al servidor DNS.

La siguiente herramienta permite probar la conectividad de TCP para una combinación de host y puerto. Esta herramienta se llama **tcpping** y la sintaxis es:

    tcpping.exe hostname [optional: port]

La utilidad **tcpping** indica si se puede llegar a un host y puerto específicos. Solo puede mostrar correcto si hay una aplicación que escucha en la combinación de host y puerto, y no hay acceso a la red de la aplicación para el host especificado y el puerto.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Depuración del acceso a recursos hospedados en la red virtual
Hay varias situaciones que pueden impedir que la aplicación llegue a un host y un puerto específicos. La mayoría de las veces se debe a una de estas tres causas:

* **Existe un firewall que lo impide.** Si tiene un firewall que lo impide, se agotará el tiempo de espera de TCP. El tiempo de espera de TCP es de 21 segundos en este caso. Utilice la herramienta **tcpping** para probar la conectividad. Los tiempos de espera agotados de TCP pueden deberse a muchas causas, además de los firewalls, pero comience por comprobar los firewalls. 
* **El DNS no es accesible.** El tiempo de espera de DNS es de tres segundos por cada servidor DNS. Si tiene dos servidores DNS, el tiempo de espera es de seis segundos. Utilice nameresolver para ver si funciona el DNS. Recuerde que no puede usar nslookup porque este no usa el DNS con el que se ha configurado la red virtual.
* **El intervalo de direcciones de punto a sitio no es válido.** El intervalo IP de punto a sitio debe estar en los intervalos de direcciones IP privadas RFC 1918 (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255). Si el intervalo utiliza direcciones IP externas, las cosas no funcionarán. 

Si estos elementos no resuelven su problema, busque primero elementos sencillas como: 

* ¿Aparece la puerta de enlace como activa en el Portal?
* ¿Indican los certificados que están sincronizados?
* ¿Cambió alguien la configuración de la red sin hacer clic en "Sincronizar red" en los planes de servicio de aplicaciones afectados? 

Si la puerta de enlace está inactiva, vuelva a activarla. Si los certificados no están sincronizados, vaya a la vista del plan de App Service en Integración con red virtual y haga clic en "Sincronizar red". Si sospecha que se ha producido un cambio en su configuración de red virtual que no se ha sincronizado con sus planes de App Service, haga clic en "Sincronizar red".  La operación "Sincronizar red" reiniciará todas las aplicaciones del plan de App Service integradas con esa red virtual. 

Si todo eso está bien, debe profundizar un poco más:

* ¿Hay alguna otra aplicación que utilice Integración con red virtual para tener acceso a recursos en la misma red virtual? 
* ¿Puede ir a la consola de la aplicación y usar tcpping para llegar a cualquier otro recurso de la red virtual? 

Si la respuesta a cualquiera de las anteriores preguntas es afirmativa, Integración con red virtual es correcta y el problema reside en otro lugar. Aquí la situación se complica porque no existe una forma sencilla de ver por qué no se llega a un host:puerto. Algunas de las causas son:

* tiene un firewall en el host que impide el acceso al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio. Para cruzar subredes, se requiere a menudo acceso público.
* el host de destino está inactivo;
* la aplicación está inactiva;
* la dirección IP o el nombre de host son incorrectos;
* la aplicación escucha en un puerto diferente del que se esperaba. Puede hacer coincidir el id. de proceso con el puerto de escucha usando "netstat -aon" en el host del punto de conexión. 
* los grupos de seguridad de la red están configurados de tal manera que impiden el acceso al host y al puerto de la aplicación desde su intervalo de direcciones IP de punto a sitio.

Recuerde que no sabe qué dirección IP del intervalo de direcciones IP de punto a sitio usará la aplicación, así que necesita permitir el acceso a todo el intervalo. 

Otros pasos de depuración son:

* conectarse a una VM de la red virtual e intentar acceder al recurso host:puerto desde ella. Para probar el acceso de TCP, use el comando **test-netconnection** de PowerShell. La sintaxis es:

      test-netconnection hostname [optional: -Port]

* abrir una aplicación en una VM y probar el acceso a ese host y ese puerto desde la consola de la aplicación.

#### <a name="on-premises-resources"></a>Recursos locales ####

Si la aplicación no puede conectar con un recurso local, compruebe si puede acceder al recurso desde su red virtual. Use el comando **test-netconnection** de PowerShell para comprobar el acceso de TCP. Si la máquina virtual no puede acceder al recurso local, asegúrese de que funciona la conexión VPN de sitio a sitio. Si funciona, compruebe lo mismo que se indicó antes, así como el estado y la configuración de la puerta de enlace local. 

Si la máquina virtual hospedada en la red virtual puede acceder a su sistema local, pero la aplicación no, la causa es probablemente una de las razones siguientes:

* Las rutas no están configuradas con los intervalos IP de punto a sitio en la puerta de enlace local
* Los grupos de seguridad de red están bloqueando el acceso del intervalo IP de punto a sitio.
* Los firewalls locales están bloqueando el tráfico procedente del intervalo IP de punto a sitio.


## <a name="powershell-automation"></a>Automatización de PowerShell

Puede integrar App Service con Azure Virtual Network mediante PowerShell. Para obtener un script que esté listo para ejecutarse, consulte [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3) (Conectar una aplicación de Azure App Service a una red de Azure Virtual Network).

## <a name="hybrid-connections-and-app-service-environments"></a>Conexiones híbridas y entornos de App Service
Existen tres características que permiten el acceso a los recursos hospedados en la red virtual. Son las siguientes:

* Integración con red virtual
* conexiones híbridas
* Entornos de App Service

Conexiones híbridas requiere que se instale un agente de retransmisión llamado administrador de conexiones híbridas (HCM) en la red. El HCM debe ser capaz de conectarse a Azure y también a la aplicación. Las conexiones híbridas no requieren un punto de conexión accesible de Internet entrante para la red remota, como existe una conexión VPN. El HCM solamente se ejecuta en Windows y puede tener hasta cinco instancias en ejecución para proporcionar alta disponibilidad. Sin embargo, Conexiones híbridas solo admite TCP y cada uno de sus puntos de conexión tiene que coincidir con una combinación de host:puerto específica. 

La característica App Service Environment permite ejecutar una instancia de un solo inquilino de Azure App Service en la red virtual. Si las aplicaciones están en una instancia de App Service Environment, pueden acceder a los recursos de la red virtual sin ningún paso adicional. Con App Service Environment, las aplicaciones se ejecutan en trabajos más eficaces y pueden escalar verticalmente a hasta 100 instancias de plan de App Service. Las instancias de App Service Environment funcionan con todas las características de redes, incluido ExpressRoute y los puntos de conexión de servicio.  

Aunque algunos casos de uso se solapan, ninguna de estas características puede reemplazar a las otras. Saber qué característica emplear depende de sus necesidades. Por ejemplo: 

* Si es un desarrollador y quiere ejecutar un sitio en Azure que pueda acceder a la base de datos en la estación de trabajo que suele usar, lo más fácil es usar Conexiones híbridas. 
* Si es una gran organización que desea colocar un gran número de propiedades web en la nube pública y administrarlas en su propia red, entonces le interesará App Service Environment. 
* Si tiene varias aplicaciones que necesitan tener acceso a los recursos de la red virtual, la integración con la red virtual es la mejor opción. 

Si la red virtual ya está conectada a la red local, usar Integración con red virtual o una instancia de App Service Environment es una manera fácil de consumir recursos locales. Si la red virtual no está conectada a la red local, resulta mucho más costoso configurar una VPN de sitio a sitio con la red virtual, en comparación con la instalación de HCM. 

Además de las diferencias funcionales, existen también diferencias de precio. La característica Entorno de App Service es un servicio premium, pero ofrece la mayor variedad de configuraciones de red, además de otras características interesantes. Integración con red virtual se puede usar con el plan de App Service Estándar o Premium y es ideal para consumir de forma segura recursos de la red virtual desde App Service con varios inquilinos. Actualmente, Conexiones híbridas depende de un cuenta de BizTalk con niveles de precios que comienzan de forma gratuita y progresivamente van aumentando según la cantidad que necesite. Sin embargo, cuando se necesita trabajar en muchas redes, no hay ninguna característica como Conexiones híbridas, que permite acceder a recursos de más de 100 redes distintas. 

## <a name="new-vnet-integration"></a>Nueva integración con red virtual ##

Hay una nueva versión de la funcionalidad Integración con red virtual que no depende de la tecnología de VPN de punto a sitio. A diferencia de la característica ya existente, la nueva característica en vista previa funcionará con ExpressRoute y los puntos de conexión de servicio. 

La nueva funcionalidad solo está disponible en las unidades de escalado más recientes de Azure App Service. Si puede escalar a PremiumV2, está en una unidad de escalado más reciente de App Service. La UI de Integración con red virtual del portal le indicará si la aplicación puede usar la nueva característica Integración con red virtual. 

La nueva versión está en versión preliminar y tiene las siguientes características.

* No es necesaria ninguna puerta de enlace para usar la nueva característica de integración con red virtual.
* Puede acceder a recursos en conexiones de ExpressRoute sin ninguna configuración adicional, aparte de la integración con la red virtual conectada a ExpressRoute.
* La aplicación y la red virtual deben estar en la misma región
* La nueva característica requiere una subred sin usar en la red virtual de Resource Manager.
* El plan de App Service debe ser un plan Estándar, Premium o PremiumV2.
* No se admiten en la nueva característica de las cargas de trabajo de producción mientras se encuentra en versión preliminar.
* La aplicación debe estar en una implementación de Azure App Service que se pueda escalar verticalmente a Premium v2.
* La nueva característica Integración con red virtual no funciona con aplicaciones en App Service Environment.
* No puede eliminar una red virtual con una aplicación integrada.  
* Las tablas de rutas y el emparejamiento global no están aún disponibles con la nueva integración con red virtual.  
* Se usa una dirección para cada instancia del plan de App Service. Puesto que no se puede cambiar el tamaño de la subred después de la asignación, use una subred que pueda cubrir un tamaño mayor al de su escala máxima. El tamaño /27 con 32 direcciones es el recomendado, ya podría dar cabida a un plan de App Service escalado a 20 instancias.
* Para consumir los recursos protegidos del punto de conexión de servicio, use la nueva funcionalidad Integración con red virtual. Para ello, habilite los puntos de conexión de servicio en la subred usada para la integración con red virtual.

Para usar la nueva característica:

1. Vaya a la interfaz de usuario de Redes en el portal. Si la aplicación es capaz de usar la nueva característica, verá una funcionalidad para usar la nueva característica en vista previa.  

   ![Seleccione la nueva característica de en vista previa de Integración con red virtual][6]

1. Seleccione **Agregar VNET (versión preliminar)**.  

1. Seleccione la red virtual de Resource Manager con la que quiere realizar la integración y, a continuación, cree una nueva subred o elija una subred vacía existente. La integración tarda menos de un minuto en completarse. Durante la integración, la aplicación se reinicia.  Cuando se complete la integración, verá detalles sobre la red virtual con la que se ha integrado y un banner en la parte superior, que indica que la característica está en versión preliminar.

   ![Selección de la red virtual y la subred][7]

Para habilitar la aplicación, use el servidor DNS con el que se ha configurado la red virtual, cree una configuración de la aplicación para la aplicación, donde el nombre sea WEBSITE_DNS_SERVER y el valor sea la dirección IP del servidor.  Si tiene un servidor DNS secundario, cree otra configuración de la aplicación, donde el nombre sea WEBSITE_DNS_ALT_SERVER y el valor sea la dirección IP del servidor. 

Para desconectar la aplicación de la red virtual, seleccione **Desconectar**. Esta acción reiniciará la aplicación web. 

La nueva característica Integración con red virtual le permite usar puntos de conexión de servicio.  Para usar puntos de conexión de servicio con la aplicación, use la nueva integración con red virtual para conectarse a una red virtual seleccionada y, a continuación, configure los puntos de conexión de servicio en la subred que usó para la integración. 

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
