---
title: Integración de Azure Functions con una red virtual de Azure
description: Tutorial paso a paso donde se explica cómo conectarse a una función en una red virtual de Azure
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 12/03/2018
ms.author: alkarche
ms.openlocfilehash: 042d41e9125e5d4fa0af04fd6edeba5f0b33123a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001585"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Integración de una aplicación de función con una red virtual de Azure
En este tutorial paso a paso, se explica cómo se utiliza Azure Functions para conectarse a recursos de una red virtual de Azure. 

En este tutorial, implementaremos un sitio de Wordpress de una máquina virtual en una red virtual privada que no es accesible desde Internet. A continuación, implementaremos una función que tenga acceso a Internet y a la red virtual. Utilizaremos esa función para acceder a recursos del sitio de Wordpress implementado en la red virtual.

Para más información sobre el funcionamiento del sistema, la resolución de problemas y la configuración avanzada, consulte el documento [Integración de su aplicación con una instancia de Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). En el plan de servicio dedicado, Azure Functions tiene las mismas funcionalidades de hospedaje que las aplicaciones web, por lo que toda la funcionalidad y las limitaciones del documento se aplican también a Functions.

## <a name="topology"></a>Topología
 ![Interfaz de usuario de Integración con red virtual][1]

## <a name="creating-a-vm-inside-of-a-vnet"></a>Creación de una máquina virtual en una red virtual

Para empezar, vamos a crear una máquina virtual preconfigurada que ejecute Wordpress en una red virtual. 

Hemos decidido instalar Wordpress en una máquina virtual porque es uno de los recursos más económicos que se pueden implementar dentro de una red virtual. Tenga en cuenta que este escenario también puede funcionar con cualquier recurso de las redes virtuales, como las API REST, otros servicios de Azure, diferentes entornos de App Service, etc.

1.  Vaya a Azure Portal.
2.  Agregue un nuevo recurso; para ello, abra la hoja "Crear un recurso".
3.  Busque "[Wordpress LEMP7 Max Performance on CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)" y abra la hoja de creación. 
4.  En la hoja de creación, configure la máquina virtual con la siguiente información:
    1.  Cree un nuevo grupo de recursos para que esta máquina virtual pueda limpiar los recursos con más facilidad al final del tutorial. El nombre que asigné al grupo de recursos es "Function-VNET-Tutorial".
    1.  Proporcione un nombre único a la máquina virtual. La mía se llama "VNET-Wordpress".
    1.  Seleccione la región más cercana a la suya.
    1.  Seleccione el tamaño como B1s (1 vcpu y 1 GB de memoria).
    1.  En la cuenta de administrador, elija la autenticación de contraseña y escriba un nombre de usuario único y una contraseña. En este tutorial, no es necesario iniciar sesión en la máquina virtual a menos que deba solucionar algún problema.
    
        <img src="./media/functions-create-vnet/create-VM-1.png" width="700">

1. Vaya a la pestaña Redes y especifique la siguiente información:
    1.  Cree una nueva red virtual.
    1.  Especifique el intervalo de direcciones privadas que desee y una subred de dicho intervalo de direcciones. El tamaño de la subred determinará cuántas máquinas virtuales pueden utilizarse en el plan de App Service. Si no tiene mucha experiencia con las direcciones IP y las subredes, consulte este [documento con los aspectos básicos](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Las direcciones IP y las subredes son importantes en este escenario, por lo que es conveniente que lea algunos artículos y vea vídeos en línea hasta que le resulten familiares. 
        1. En este ejemplo, he decidido utilizar la red 10.10.0.0/16 con la subred 10.10.1.0/24. He preferido aprovisionar en exceso y utilizar una subred /16 porque resulta fácil calcular qué subredes están disponibles en la red 10.10.0.0/16.
        
        <img src="./media/functions-create-vnet/create-VM-2.png" width="700">

1. Vuelva a la pestaña Redes y establezca la IP pública en "Ninguna". De este modo, la VM se implementará solo con acceso a la red virtual.
       
    <img src="./media/functions-create-vnet/create-VM-2.1.png" width="700">

7. Cree la máquina virtual. Este proceso puede tardar 5 minutos, aproximadamente.
8. Cuando se cree la máquina virtual, visite la pestaña Redes y anote la dirección IP privada para utilizarla más adelante. La máquina virtual no debe tener una dirección IP pública.

    ![14]

Ahora, tendrá un sitio de Wordpress implementado por completo dentro de la red virtual. Este sitio no será accesible desde la red pública de Internet.

## <a name="create-a-dedicated-function-app"></a>Creación de una aplicación de función dedicada

El siguiente paso consiste en crear una aplicación de función dentro de un plan de App Service estándar o superior. Tenga en cuenta que las aplicaciones de funciones del plan de consumo no admiten la integración con redes virtuales.

1. Vaya a Azure Portal.
2. Agregue un nuevo recurso; para ello, abra la hoja "Crear un recurso".
3. Seleccione "Serverless Function App" (Function App sin servidor).
4. Escriba todos los datos habituales en la hoja de creación, con una excepción:
    1. Seleccione un nivel estándar o superior en el plan de App Service.

        <img src="./media/functions-create-vnet/Create-App-Service-Plan.PNG" width="300">
    
1. Este es el aspecto de mi hoja de creación una vez completada:

    <img src="./media/functions-create-vnet/Create-Function-App.png" width="300">


## <a name="connect-your-function-app-to-your-vnet"></a>Conexión de la aplicación de función a la red virtual

Ahora, tenemos un sitio de Wordpress que hospeda muchos archivos dentro de una red virtual y necesitamos conectar la aplicación de función a esa red virtual.

1.  En el portal de la aplicación de función del paso anterior, seleccione **Características de la plataforma** y **Redes**.
1.  Bajo Integración de VNET, seleccione **Haga clic para configurar**.

    <img src="./media/functions-create-vnet/Networking-1.png" width="450">

1. En la página de integración de la red virtual, seleccione **Agregar VNET (versión preliminar)**.

    <img src="./media/functions-create-vnet/Networking-2.png" width="600"> 
    
1.  Cree una nueva subred para la función y el plan de App Service que desea usar. Tenga en cuenta que el tamaño de la subred limitará el número total de máquinas virtuales que se pueden agregar al plan de App Service. La red virtual enrutará el tráfico automáticamente entre las subredes de la red virtual, por lo que no importa que la función esté en una subred distinta a la de la máquina virtual. 
    
    <img src="./media/functions-create-vnet/Networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-vnet"></a>Creación de una función con acceso a un recurso de la red virtual

Ahora, la aplicación de función puede tener acceso a la red virtual con el sitio de Wordpress, así que vamos a usar la función para acceder al archivo y enviarlo al usuario. En este ejemplo, utilizaremos un sitio de Wordpress como API y un proxy de la función como función de llamada, ya que los dos resultan fáciles de configurar y visualizar. También podría utilizar cualquier otra API implementada en una red virtual y otra función con código que realizara llamadas a dicha API. Un servidor SQL Server implementado en la red virtual sería el ejemplo perfecto.

1. En el portal, abra la aplicación de función del paso anterior.
1. Cree un proxy; para ello, seleccione **Proxies** > **+**.

    <img src="./media/functions-create-vnet/New-Proxy.png" width="250">

1. Especifique el nombre del proxy y la ruta. Mi ruta es /plant.
1. Especifique la dirección IP del sitio de Wordpress anterior y establezca la dirección URL de back-end en `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`.
    
    <img src="./media/functions-create-vnet/Create-Proxy.png" width="900">

Ahora, si intentara visitar directamente la dirección URL del back-end pegándola en una nueva pestaña del explorador, la página debería agotar el tiempo de espera. No resulta sorprendente, ya que el sitio de Wordpress solamente está conectado a la red virtual y no a Internet. Si pega la dirección URL del proxy en el explorador, debería aparecer una bonita imagen de una planta, que se ha extraído del sitio de Wordpress que está en la red virtual. 

La aplicación de función está conectada a Internet y a la red virtual. El proxy recibe una solicitud a través de la red pública de Internet y, a continuación, actúa como un proxy HTTP y reenvía la solicitud a la red virtual. Después, el proxy transmite la respuesta de vuelta a través de Internet. 

<img src="./media/functions-create-vnet/Plant.png" width="900">

## <a name="next-steps"></a>Pasos siguientes

Cuando Azure Functions se ejecuta en planes de App Service, lo hace en el mismo servicio que las aplicaciones web, por lo que toda la documentación de Web Apps es aplicable a las funciones dedicadas.

1. [Más información sobre la integración de la red virtual con App Service y Functions](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
1. [Más información sobre las redes virtuales en Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)
1. [Habilitación del control y las características de red con App Service Environment](https://docs.microsoft.com/azure/app-service/environment/intro)
1. [Conexión a recursos locales específicos mediante conexiones híbridas sin modificar el firewall](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)
1. [Más información sobre Function Proxies](https://review.docs.microsoft.com/azure/azure-functions/functions-proxies)

<!--Image references-->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/Create-Function-App.png
[3]: ./media/functions-create-vnet/Create-App-Service-Plan.PNG
[4]: ./media/functions-create-vnet/configure-VNET.png
[5]: ./media/functions-create-vnet/create-VM-1.png
[6]: ./media/functions-create-vnet/create-VM-2.png
[7]: ./media/functions-create-vnet/create-VM-2.1.png
[8]: ./media/functions-create-vnet/Networking-1.png
[9]: ./media/functions-create-vnet/Networking-2.png
[10]: ./media/functions-create-vnet/Networking-3.png
[11]: ./media/functions-create-vnet/New-Proxy.png
[12]: ./media/functions-create-vnet/Create-Proxy.png
[14]: ./media/functions-create-vnet/VM-Networking.png
