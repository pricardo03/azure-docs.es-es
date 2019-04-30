---
title: Integración de Azure Functions con Azure virtual network
description: Un tutorial paso a paso que se muestra cómo conectarse a una función a una red virtual de Azure
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 96ab479d3373eb6e575a00898f7007a4df252e39
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125676"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Integrar una aplicación de función con una red virtual de Azure

Este tutorial muestra cómo usar Azure Functions para conectarse a recursos en una red virtual de Azure.

Para este tutorial, se va a implementar un sitio de WordPress en una máquina virtual en una red virtual que no sea accesible desde internet. A continuación, implementaremos una función con acceso a internet y la red virtual. Vamos a usar esa función para acceder a recursos desde el sitio de WordPress que se implementan dentro de la red virtual.

Para obtener más información sobre cómo funciona el sistema, consulte solución de problemas y configuración avanzada, [integrar su aplicación con una red virtual](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Azure functions en el plan Premium tienen las mismas capacidades de hospedaje, como aplicaciones web, por lo que todas las funcionalidades y limitaciones de este artículo se aplican a funciones.

## <a name="topology"></a>Topología

 ![Interfaz de usuario para la integración de red virtual][1]

## <a name="create-a-vm-inside-a-virtual-network"></a>Crear una máquina virtual dentro de una red virtual

Para empezar, vamos a crear una máquina virtual preconfigurada que se ejecute WordPress en una red virtual. 

Hemos elegido WordPress en una máquina virtual porque es uno de los recursos más económicos que se pueden implementar en una red virtual. Tenga en cuenta que este escenario también puede trabajar con cualquier recurso en una red virtual, como las API de REST, entornos de App Service y otros servicios de Azure.

1. Vaya a Azure Portal.
2. Agregar un nuevo recurso abriendo el **crear un recurso** hoja.
3. Busque "[WordPress LEMP7 máximo rendimiento en CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)" y abra su hoja de creación. 
4. En el **Fundamentos** pestaña, configure la máquina virtual con la siguiente información:
    1. Cree un nuevo grupo de recursos para que esta máquina virtual pueda limpiar los recursos con más facilidad al final del tutorial. En este caso, vamos a usar "Tutorial de red virtual de función" como ejemplo.
    1. Proporcione un nombre único a la máquina virtual. Vamos a usar "Red virtual de Wordpress" como ejemplo.
    1. Seleccione la región más cercana a usted.
    1. Seleccione el tamaño como B1s (1 vCPU y 1 GB de memoria).
    1. En la cuenta de administrador, elija la autenticación de contraseña y escriba un nombre de usuario único y una contraseña. Para este tutorial, no tendrá que iniciar sesión en la máquina virtual, a menos que deba solucionar el problema.
    
        ![Pestaña aspectos básicos para crear una máquina virtual](./media/functions-create-vnet/create-vm-1.png)

1. Mover a la **redes** pestaña y escriba la siguiente información:
    1.  Crear una nueva red virtual.
    1.  Escriba un intervalo de direcciones privadas y una subred dentro de ese intervalo de direcciones. El tamaño de la subred determinará cuántas máquinas virtuales pueden utilizarse en el plan de App Service. Si la subred y el direccionamiento IP sean nuevo para usted, hay un [documento que trata los aspectos básicos](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Direccionamiento IP y las subredes son importantes en este escenario, por lo que recomendamos que lea algunos artículos y vea algunos vídeos en línea hasta que tiene sentido. 
    
        En este ejemplo, nos estamos optando por utilizar la red 10.10.0.0/16 con una subred de 10.10.1.0/24. Estamos aprovisionamiento en exceso y utilizando un /16 subred porque es muy fácil calcular qué subredes están disponibles en la red 10.10.0.0/16.
        
        <img src="./media/functions-create-vnet/create-vm-2.png" width="700">

1. En el **redes** pestaña, establezca la dirección IP pública en **ninguno**. Este paso implementará la VM con acceso a solo la red virtual.
       
    <img src="./media/functions-create-vnet/create-vm-2-1.png" width="700">

7. Cree la máquina virtual. El proceso tardará aproximadamente 5 minutos.
8. Una vez creada la máquina virtual, vaya a su **redes** pestaña y anote la dirección IP privada para su uso posterior. La máquina virtual no debe tener una dirección IP pública.

    ![14]

Ahora tiene un sitio de WordPress que se implementa completamente dentro de la red virtual. Este sitio no será accesible desde la red pública de Internet.

## <a name="create-a-function-app-in-a-premium-plan"></a>Crear una aplicación de función en un plan Premium

El siguiente paso es crear una aplicación de función en un plan Premium. Un plan Premium aporta escala sin servidor con todas las ventajas de un plan de App Service dedicado. Aplicaciones de función creadas mediante el plan de consumo no admiten la integración de red virtual.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

## <a name="connect-your-function-app-to-your-virtual-network"></a>Conectar la aplicación de función a la red virtual

Con un sitio de WordPress que hospeda los archivos desde dentro de la red virtual, ahora puede conectar la aplicación de función a la red virtual.

1.  En el portal para la aplicación de función en el paso anterior, seleccione **características de la plataforma**. A continuación, seleccione **redes**.

    <img src="./media/functions-create-vnet/networking-0.png" width="850">

1.  Seleccione **haga clic aquí para configurar** en **integración con red virtual**.

    ![Estado de configuración de una característica de red](./media/functions-create-vnet/Networking-1.png)

1. En la página de integración de red virtual, seleccione **agregar red virtual (versión preliminar)**.

    <img src="./media/functions-create-vnet/networking-2.png" width="600"> 
    
1.  Crear una nueva subred para la función y el plan de App Service para usar. Tenga en cuenta que el tamaño de subred restringirá el número total de máquinas virtuales que se pueden agregar al plan de App Service. La red virtual enrutará automáticamente el tráfico entre las subredes de la red virtual, por lo que no importa que la función está en una subred distinta de la máquina virtual. 
    
    <img src="./media/functions-create-vnet/networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-virtual-network"></a>Creación de una función que tiene acceso a un recurso en la red virtual

Ahora la aplicación de función puede tener acceso a la red virtual con nuestro sitio de WordPress. Así que vamos a usar la función para acceder al archivo y enviarlos al usuario. En este ejemplo, vamos a usar un sitio de WordPress como la API y un proxy que la función que realiza la llamada debido a que son fáciles de configurar y visualizar. 

Puede usar fácilmente cualquier otra API implementado dentro de una red virtual. También puede usar otra función con el código que realiza llamadas de API a la API implementada dentro de la red virtual. Una instancia de SQL Server implementada dentro de la red virtual es un ejemplo perfecto.

1. En el portal, abra la aplicación de función en el paso anterior.
1. Crear un proxy seleccionando **Proxies** > **+**.

    <img src="./media/functions-create-vnet/new-proxy.png" width="250">

1. Configurar la ruta y nombre de proxy. En este ejemplo se usa "/ Plante" como una ruta.
1. Rellene la IP de su sitio de WordPress desde anteriormente y establezca **dirección URL de back-end** a `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`
    
    <img src="./media/functions-create-vnet/create-proxy.png" width="900">

Ahora, si se intenta visitar la dirección URL de back-end directamente pegándolos en una nueva pestaña del explorador, la página debe tiempo de espera. Esto es porque está conectado el sitio de WordPress a solo la red virtual y no por internet. Si pega la dirección URL del proxy en el explorador, verá una imagen de la planta (extraída de su sitio de WordPress) dentro de la red virtual. 

La aplicación de función está conectada a internet y la red virtual. El proxy recibe una solicitud a través de la red pública de Internet y, a continuación, actúa como un proxy HTTP y reenvía la solicitud a la red virtual. Después, el proxy transmite la respuesta de vuelta a través de Internet. 

<img src="./media/functions-create-vnet/plant.png" width="900">

## <a name="next-steps"></a>Pasos siguientes

Las funciones que se ejecutan en un plan Premium comparten la misma infraestructura subyacente de App Service como aplicaciones web en los planes de PremiumV2. Toda la documentación para las aplicaciones web se aplica a las funciones del plan Premium.

* [Más información sobre las opciones de red de funciones](./functions-networking-options.md)
* [Leer las funciones de preguntas más frecuentes de red](./functions-networking-faq.md)
* [Más información sobre redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilitar más características de red y control con entornos de App Service](../app-service/environment/intro.md)
* [Conectarse a recursos individuales de forma local sin cambios en el firewall mediante conexiones híbridas](../app-service/app-service-hybrid-connections.md)
* [Más información sobre Functions Proxies](./functions-proxies.md)

<!--Image references -->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/create-function-app.png
[3]: ./media/functions-create-vnet/create-app-service-plan.png
[4]: ./media/functions-create-vnet/configure-vnet.png
[5]: ./media/functions-create-vnet/create-vm-1.png
[6]: ./media/functions-create-vnet/create-vm-2.png
[7]: ./media/functions-create-vnet/create-vm-2-1.png
[8]: ./media/functions-create-vnet/networking-1.png
[9]: ./media/functions-create-vnet/networking-2.png
[10]: ./media/functions-create-vnet/networking-3.png
[11]: ./media/functions-create-vnet/new-proxy.png
[12]: ./media/functions-create-vnet/create-proxy.png
[14]: ./media/functions-create-vnet/vm-networking.png
