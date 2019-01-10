---
title: Restricción de direcciones IP de cliente en Azure App Service | Microsoft Docs
description: Uso de restricciones de IP con Azure App Service
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 337d71c84ace7f44c2668cf2344d9083c4a85bee
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651069"
---
# <a name="azure-app-service-static-ip-restrictions"></a>Restricciones de IP estáticas de Azure App Service #

Las restricciones de direcciones IP le permiten definir una lista de direcciones IP ordenadas por prioridad que tienen permiso para acceder a la aplicación. La lista de elementos permitidos puede incluir direcciones IPv4 e IPv6. Cuando hay una o varias entradas, hay una denegación implícita de todo lo que existe al final de la lista. 

La funcionalidad Restricciones de IP funciona con todas las cargas de trabajo hospedadas en App Service, lo que incluye aplicaciones web, aplicaciones de API, aplicaciones de Linux, aplicaciones de contenedor de Linux e instancias de Functions. 

Cuando se realiza una solicitud a la aplicación, la dirección IP de origen se evalúa con respecto a la lista de restricciones de direcciones IP. Si la dirección no tiene permitido el acceso según las reglas de la lista, el servicio responde con un código de estado [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

La funcionalidad de restricciones de direcciones IP se implementa en los roles front-end de App Service, que son de nivel superior de los hosts de trabajo donde el código se ejecuta. Por lo tanto, las restricciones de IP son efectivamente las listas de control de acceso de red.  

![Flujo de restricciones de direcciones IP](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Durante un tiempo, la funcionalidad de restricciones de direcciones IP en el portal estuvo un nivel por encima de la funcionalidad de ipSecurity en IIS. La funcionalidad actual es diferente. Todavía puede configurar ipSecurity dentro de su archivo web.config de la aplicación, pero, antes de que todo el tráfico llegue a IIS, se aplicarán las reglas de restricciones de direcciones IP basadas en front-end.

## <a name="adding-and-editing-ip-restriction-rules-in-the-portal"></a>Adición y edición de reglas de restricciones de direcciones IP en el portal ##

Para agregar una regla de restricción de IP a la aplicación, use el menú para abrir **Red**>**Restricciones de IP** y haga clic en **Configurar restricciones de IP**.

![Opciones de red de App Service](media/app-service-ip-restrictions/ip-restrictions.png)  

Desde la interfaz de las restricciones de direcciones IP, puede revisar la lista de reglas de restricción de IP definidas para la aplicación.

![lista de restricciones de IP](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Si se han configurado las reglas como se muestra en esta imagen, la aplicación solo aceptaría el tráfico de 131.107.159.0/24 y se denegaría desde cualquier otra dirección IP.

Puede hacer clic en **[+] Agregar** para agregar una nueva regla de restricciones de IP. Una vez que agregue una regla, entrará en vigor de inmediato. Las reglas se aplican en orden de prioridad, empezando por el número más bajo y en ascenso. Hay una denegación implícita de todo lo que esté en vigor una vez que agregue incluso una sola regla. 

![Adición de una regla de restricción de direcciones IP](media/app-service-ip-restrictions/ip-restrictions-add.png)

La notación de dirección IP debe ser CIDR tanto para direcciones IPv4 como IPv6. Para especificar una dirección exacta, puede usar un formato como 1.2.3.4/32, donde los cuatro primeros octetos representan la dirección IP y /32 es la máscara. La notación CIDR IPv4 para todas las direcciones es 0.0.0.0/0. Para más información acerca de la notación CIDR, puede leer [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) (Enrutamiento interdominios sin clases).  

Puede hacer clic en cualquier fila para editar una regla de restricción de direcciones IP existente. Las modificaciones son efectivas inmediatamente, incluidos los cambios en el orden de prioridad.

![Edición de una regla de restricción de direcciones IP](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Para eliminar una regla, haga clic en los puntos suspensivos **...**  en la regla y, a continuación, haga clic en **quitar**. 

![eliminación de una regla de restricción de direcciones IP](media/app-service-ip-restrictions/ip-restrictions-delete.png)

## <a name="programmatic-manipulation-of-ip-restriction-rules"></a>Manipulación mediante programación de reglas de restricción de direcciones IP ##

Actualmente no hay ninguna CLI ni PowerShell para la nueva funcionalidad de restricciones de direcciones IP, pero los valores pueden establecerse manualmente con una operación PUT en la configuración de la aplicación en Resource Manager. Por ejemplo, puede usar resources.azure.com y editar el bloque ipSecurityRestrictions para agregar el código JSON requerido. 

La ubicación de esta información en Resource Manager es:

management.azure.com/subscriptions/**Id. de suscripción**/resourceGroups/**resource groups**/providers/Microsoft.Web/sites/**nombre de aplicación web**/config/web?api-version=2018-02-01

La sintaxis JSON para el ejemplo anterior es:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],
