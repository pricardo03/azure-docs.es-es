---
title: Restricción de direcciones IP de cliente en Azure App Service | Microsoft Docs
description: Cómo usar restricciones de acceso con Azure App Service
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
ms.openlocfilehash: bb6ab29f02282a394e3f93e41682ceaec5208b75
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59357620"
---
# <a name="azure-app-service-static-access-restrictions"></a>Restricciones de acceso estático de Azure App Service #

Restricciones de acceso le permiten definir una prioridad de permitir o denegar lista ordenada de direcciones IP que tienen permiso para acceder a la aplicación. La lista de elementos permitidos puede incluir direcciones IPv4 e IPv6. Cuando hay una o varias entradas, hay una denegación implícita de todo lo que existe al final de la lista.

La funcionalidad de las restricciones de acceso funciona con todas las que cargas de trabajo, que incluyen; hospedada en App Service aplicaciones Web, aplicaciones de API, aplicaciones de Linux, aplicaciones de contenedor de Linux y funciones.

Cuando se realiza una solicitud a la aplicación, la dirección de IP se evalúa con respecto a la lista de restricciones de acceso. Si la dirección no tiene permitido el acceso según las reglas de la lista, el servicio responde con un código de estado [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

La funcionalidad de las restricciones de acceso se implementa en los roles de front-end de App Service, que son de nivel superior de los hosts de trabajo donde se ejecuta el código. Por lo tanto, las restricciones de acceso son eficazmente las ACL de red.  

![flujo de las restricciones de acceso](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Durante un tiempo, la funcionalidad de las restricciones de acceso en el portal era un nivel por encima de la capacidad de ipSecurity en IIS. La funcionalidad de las restricciones de acceso actual es diferente. Todavía puede configurar ipSecurity dentro de su archivo web.config de la aplicación, pero antes de que todo el tráfico llega a IIS, se aplicarán las reglas de las restricciones de acceso en función de front-end.

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Adición y edición de reglas de restricción de acceso en el portal ##

Para agregar una regla de restricción de acceso a la aplicación, use el menú para abrir **red**>**restricciones de acceso** y haga clic en **configurar restricciones de acceso**

![Opciones de red de App Service](media/app-service-ip-restrictions/ip-restrictions.png)  

Desde la interfaz de usuario de las restricciones de acceso, puede revisar la lista de reglas de restricción de acceso definido para la aplicación.

![restricciones de acceso de la lista](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Si se han configurado las reglas como se muestra en esta imagen, la aplicación solo aceptaría el tráfico de 131.107.159.0/24 y se denegaría desde cualquier otra dirección IP.

Puede hacer clic en **[+] agregar** para agregar una nueva regla de restricción de acceso. Una vez que agregue una regla, entrará en vigor de inmediato. Las reglas se aplican en orden de prioridad, empezando por el número más bajo y en ascenso. Hay una denegación implícita de todo lo que esté en vigor una vez que agregue incluso una sola regla.

![Agregar una regla de restricción de acceso](media/app-service-ip-restrictions/ip-restrictions-add.png)

La notación de dirección IP debe ser CIDR tanto para direcciones IPv4 como IPv6. Para especificar una dirección exacta, puede usar un formato como 1.2.3.4/32, donde los cuatro primeros octetos representan la dirección IP y /32 es la máscara. La notación CIDR IPv4 para todas las direcciones es 0.0.0.0/0. Para más información acerca de la notación CIDR, puede leer [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) (Enrutamiento interdominios sin clases).  

Puede hacer clic en cualquier fila para editar una regla de restricción de acceso existente. Las modificaciones son efectivas inmediatamente, incluidos los cambios en el orden de prioridad.

![Editar una regla de restricción de acceso](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Para eliminar una regla, haga clic en los puntos suspensivos **...**  en la regla y, a continuación, haga clic en **quitar**.

![Eliminar regla de restricción de acceso](media/app-service-ip-restrictions/ip-restrictions-delete.png)

También puede restringir el acceso de implementación en la ficha siguiente. Para agregar, editar o eliminar cada regla, siga el paso anterior.

![restricciones de acceso de la lista](media/app-service-ip-restrictions/ip-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Manipulación de las reglas de restricción de acceso mediante programación ##

Actualmente no hay ningún CLI o PowerShell para la nueva funcionalidad de las restricciones de acceso, pero se pueden establecer manualmente los valores con una operación PUT en la configuración de la aplicación en el Administrador de recursos. Por ejemplo, puede usar resources.azure.com y editar el bloque ipSecurityRestrictions para agregar el código JSON requerido.

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

## <a name="function-app-ip-restrictions"></a>Restricciones de IP de la aplicación de función

Las restricciones de IP están disponibles para ambas aplicaciones de función con la misma funcionalidad que los planes de App Service. Tenga en cuenta que se habilita la IP restricciones se deshabilitará al editor de código de portal para las direcciones IP no permitido.

[Obtenga más información aquí](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)