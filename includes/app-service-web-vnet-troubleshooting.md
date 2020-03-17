---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: 2d2a82552a846cedfa5da3bb6ec6df8a40b67732
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671492"
---
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
* ¿Hay un NSG que bloquea la salida de la subred de integración?
* Si es a través de una VPN o ExpressRoute, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si puede acceder a los puntos de conexión de la red virtual pero no del entorno local, compruebe las rutas.
* ¿Tiene permisos suficientes para configurar la delegación en la subred de integración? Al configurar la versión de Integración con red virtual que necesita una puerta de enlace, la subred de integración se delegará en Microsoft.Web. La interfaz de usuario de integración de la red virtual delegará la subred en Microsoft.Web automáticamente. Si la cuenta no tiene suficientes permisos de red para establecer la delegación, necesitará que alguien que pueda configurar atributos en la subred de integración delegue la subred. Para delegar manualmente la subred de integración, vaya a la interfaz de usuario de la subred de Azure Virtual Network y establezca delegación para Microsoft.Web. 

**Integración con red virtual con requisito de puerta de enlace**
* ¿Es el intervalo de direcciones de punto a sitio en los intervalos de RFC 1918 (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255)?
* ¿Aparece la puerta de enlace como activa en el Portal? Si la puerta de enlace está inactiva, vuelva a activarla.
* ¿Aparecen los certificados como sincronizados o sospecha que la configuración de red ha cambiado?  Si los certificados no están sincronizados o sospecha que se ha producido un cambio en la configuración de red virtual que no se ha sincronizado con sus planes de App Service, haga clic en "Sincronizar red".
* Si se usa una VPN, ¿está configurada la puerta de enlace local para enrutar el tráfico de vuelta a Azure? Si puede acceder a los puntos de conexión de la red virtual pero no del entorno local, compruebe las rutas.
* ¿Está intentando usar una puerta de enlace de coexistencia que admite tanto una conexión de punto a sitio como ExpressRoute? Las puertas de enlace de coexistencia no son compatibles con la característica Integración con red virtual.

Depurar problemas de red es todo un reto porque no se puede ver lo que está bloqueando el acceso en una combinación de host y puerto específica. Algunas de las causas son:

* Tiene un firewall en el host que evita el acceso al puerto de la aplicación desde el intervalo IP de punto a sitio. Para cruzar subredes, se requiere a menudo acceso público.
* El host de destino está fuera de servicio.
* La aplicación está fuera de servicio.
* La dirección IP o el nombre de host son incorrectos.
* La aplicación escucha en un puerto diferente al esperado. Puede hacer coincidir el id. de proceso con el puerto de escucha usando "netstat -aon" en el host del punto de conexión. 
* Los grupos de seguridad de red están configurados de tal manera que evitan el acceso al host y al puerto de la aplicación desde el intervalo IP de punto a sitio.

Recuerde que no sabe qué dirección va a usar la aplicación realmente. Podría ser cualquier dirección en el intervalo de direcciones de punto a sitio o de subred de integración, por lo que será necesario permitir el acceso desde el intervalo de direcciones completo. 

Otros pasos de depuración son:

* Conectarse a una máquina virtual de la red virtual e intentar acceder al recurso host:puerto desde allí. Para probar el acceso de TCP, use el comando **test-netconnection** de PowerShell. La sintaxis es:

      test-netconnection hostname [optional: -Port]

* Abra una aplicación en una máquina virtual y pruebe el acceso a ese host y ese puerto desde la consola de la aplicación con **tcpping**.

#### <a name="on-premises-resources"></a>Recursos locales ####

Si la aplicación no puede conectar con un recurso local, compruebe si puede acceder al recurso desde su red virtual. Use el comando **test-netconnection** de PowerShell para comprobar el acceso de TCP. Si la máquina virtual no puede acceder al recurso local, puede que la conexión de ExpressRoute o red virtual no esté configurada correctamente.

Si la máquina virtual hospedada en la red virtual puede acceder a su sistema local, pero la aplicación no, la causa es probablemente una de las razones siguientes:

* Las rutas no están configuradas con los intervalos de direcciones de punto a sitio o subred en la puerta de enlace local.
* Los grupos de seguridad de red están bloqueando el acceso del intervalo IP de punto a sitio.
* Los firewalls locales están bloqueando el tráfico del intervalo IP de punto a sitio.
* Está intentando acceder a una dirección que no es de RFC 1918 mediante la característica Integración con red virtual regional.