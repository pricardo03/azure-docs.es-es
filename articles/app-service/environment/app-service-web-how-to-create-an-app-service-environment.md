---
title: Creación de una instancia de App Service Environment v1 en Azure
description: Descripción del flujo de creación de una instancia de App Service Environment v1
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 9bc796c4d0d449f72dc3234bc2825554eafaf77f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128887"
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Creación de una instancia de App Service Environment v1 

> [!NOTE]
> Este artículo trata sobre App Service Environment v1. Hay una versión más reciente que resulta más fácil de usar y se ejecuta en una infraestructura más eficaz. Para aprender más sobre la nueva versión, empiece por consultar la [Introducción a App Service Environment](intro.md).
> 

### <a name="overview"></a>Información general
App Service Environment (ASE) es una opción del servicio Premium de Azure App Service que ofrece una mejor funcionalidad de configuración que no está disponible en las marcas multiinquilino. La característica de ASE esencialmente implementa Azure App Service en la red virtual de un cliente. Para comprender mejor las funciones que ofrecen los entornos de App Service, lea el artículo donde[que explica en qué consiste un entorno de App Service][WhatisASE].

### <a name="before-you-create-your-ase"></a>Antes de crear su ASE
Es importante ser consciente de las cosas que no se pueden cambiar. Los aspectos que no se pueden cambiar sobre ASE una vez creado son:

* Location
* Subscription
* Grupo de recursos
* Red virtual usada
* La subred usada 
* Tamaño de la subred

Al seleccionar una red virtual y especificar una subred, asegúrese de que es lo suficientemente grande como para alojar el crecimiento futuro. 

### <a name="creating-an-app-service-environment-v1"></a>Creación de una instancia de App Service Environment v1
Para crear una instancia de App Service Environment v1, puede buscar ***App Service Environment v1*** en Azure Marketplace o a través de **Crear un recurso** -> **Web y móvil** -> **App Service Environment**. Para crear una instancia de ASEv1:

1. Proporcione el nombre de su ASE. El nombre que especifique para la instancia de ASE se usará en las aplicaciones creadas en esta. Si el nombre de la instancia de ASE es appsvcenvdemo, el nombre del subdominio será: *appsvcenvdemo.p.azurewebsites.net*. Por tanto, si creó una aplicación llamada *mytestapp*, se podría obtener acceso a ella en *mytestapp.appsvcenvdemo.p.azurewebsites.net*. No puede usar espacios en blanco en el nombre del ASE. Si utiliza caracteres en mayúsculas en el nombre, el nombre de dominio será la versión en minúsculas total de ese nombre. Si usa un ILB, el nombre de la instancia de ASE no se usa en su subdominio, sino que se indica explícitamente durante la creación de dicha instancia.
   
    ![][1]
2. Seleccione su suscripción. La suscripción que se utilice para su instancia de ASE también se aplicará a todas las aplicaciones que se creen en dicha instancia. No puede colocar su instancia de ASE en una red virtual que se encuentre en otra suscripción.
3. Seleccione o especifique un nuevo grupo de recursos. El grupo de recursos que se usa para su ASE debe ser el mismo que se utiliza para la red virtual. Si selecciona una red virtual existente, la selección del grupo de recursos para su instancia de ASE se actualizará para reflejar la red virtual.
   
    ![][2]
4. Realice las selecciones de red virtual y ubicación. Puede crear una red virtual nueva o seleccionar una red virtual existente. Si selecciona una red virtual nueva, puede especificar un nombre y una ubicación. La red virtual nueva tendrá el intervalo de dirección 192.168.250.0/23 y una subred denominada **predeterminada** que se define como 192.168.250.0/24. Puede seleccionar también simplemente una red virtual de Resource Manager o clásica creadas previamente. La selección del tipo de VIP determina si es posible obtener acceso directo a su ASE desde Internet (externo) o si usa un equilibrador de carga interno (ILB). Para obtener más información sobre ellos, consulte [Uso de un equilibrador de carga interno con un entorno de App Service][ILBASE]. Si selecciona un tipo de VIP externo, puede seleccionar la cantidad de direcciones IP externas que crea el sistema con fines de IPSSL. Si selecciona Interno, debe especificar el subdominio que utilizará el ASE. Se pueden implementar los ASE en redes virtuales que usen *o bien* intervalos de direcciones públicas *o bien* espacios de direcciones de RFC1918 (es decir, direcciones privadas). Para usar una red virtual con un intervalo de direcciones públicas, es necesario crear la red virtual por adelantado. Cuando seleccione una red virtual creada previamente, debe crear una nueva subred durante la creación de ASE. **No se puede usar una subred creada previamente en el portal. Puede crear un ASE con una subred creada previamente si crea su ASE con una plantilla de Resource Manager.** Para crear un ASE a partir de una plantilla, use la información de estos artículos: [Creación de un entorno de App Service a partir de una plantilla][ILBAseTemplate] y [Creación de un entorno de App Service de ILB a partir una plantilla][ASEfromTemplate].

### <a name="details"></a>Detalles
Se crea un ASE con 2 servidores front-end y 2 trabajos. Los front-end actúan como los puntos de conexión HTTP/HTTPS y envían tráfico a los trabajos que son los roles que hospedan sus aplicaciones. Puede ajustar la cantidad después de la creación de ASE y puede incluso configurar reglas de escalado automático en estos grupos de recursos. Para más información sobre el escalado manual, la administración y la supervisión de una instancia de App Service Environment, vaya aquí: [Cómo configurar App Service Environment][ASEConfig] 

Solo un ASE puede existir en la subred usada por ASE. La subred no se puede usar para cualquier elemento que no sea el ASE

### <a name="after-app-service-environment-v1-creation"></a>Después de la creación de una instancia de App Service Environment v1
Después de la creación de ASE puede ajustar:

* La cantidad de servidores front-end (mínimo: 2)
* La cantidad de trabajos (mínimo: 2)
* Cantidad de direcciones IP disponibles para SSL de IP
* Los tamaños de recursos de proceso usados por los servidores front-end o los trabajos (el tamaño mínimo de front-end es P2)

Hay más información sobre el escalado manual, la administración y la supervisión de los entornos de App Service Environment aquí: [Cómo configurar App Service Environment][ASEConfig] 

Para obtener información sobre el escalado automático, consulte aquí la siguiente guía: [Configuración la escalabilidad automática para una instancia de App Service Environment][ASEAutoscale]

Existen dependencias adicionales que no están disponibles para personalización, como la base de datos y el almacenamiento. Estas son controlados por Azure y se incluyen con el sistema. El almacenamiento del sistema admite hasta 500 GB para todo el entorno de App Service Environment, y la base de datos se ajusta con Azure según sea necesario por medio de la escala del sistema.

## <a name="getting-started"></a>Introducción
Para empezar a trabajar con App Service Environment v1, consulte [Introducción a App Service Environment v1][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
