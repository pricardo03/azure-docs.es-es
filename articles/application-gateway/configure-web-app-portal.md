---
title: 'Administrar el tráfico a aplicaciones de varios inquilinos, como App service web apps con Azure Application Gateway: Portal'
description: Este artículo proporcionan instrucciones sobre cómo configurar las aplicaciones de Azure App service web como miembros de grupo de back-end en application gateway nueva o existente.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/11/2019
ms.author: absha
ms.openlocfilehash: 4dae04c14f9132c54dcc0575ccb2841a4742a626
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831220"
---
# <a name="configure-app-service-with-application-gateway"></a>Configuración de App Service con Application Gateway

Application gateway le permite tener una aplicación Web servicio de aplicación de Azure u otros servicios de varios inquilinos como miembro del grupo de back-end. 

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> - Crear un grupo de back-end y agregarle un App Service
> - Crear configuración de HTTP y el sondeo personalizado con los modificadores "Seleccionar nombre de host" habilitados

## <a name="prerequisites"></a>Requisitos previos

- Puerta de enlace de aplicaciones: Si no tiene una puerta de enlace de aplicaciones existente, vea cómo [crear una puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- Servicio de aplicación: Si no tiene un servicio de aplicación existente, consulte [documentación de App service](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Agregar servicio de aplicación como grupo back-end

1. En el portal de Azure, abra la vista de configuración de application gateway.

2. En **grupos back-end**, haga clic en **agregar** para crear un nuevo grupo de back-end.

3. Proporcione un nombre adecuado para el grupo de back-end. 

4. En **destinos**, haga clic en la lista desplegable y elija **App Services** como la opción.

5. Una lista desplegable situada inmediatamente debajo del **destinos** aparecerá lista desplegable que contiene una lista de los servicios de aplicación. En esta lista desplegable, elija el servicio de aplicación que desea agregar como miembro del grupo back-end y haga clic en Agregar.

   ![El back-end de App service](./media/configure-web-app-portal/backendpool.png)

## <a name="create-http-settings-for-app-service"></a>Crear una configuración de HTTP para App service

1. En **configuración HTTP**, haga clic en **agregar** para crear una nueva configuración de HTTP.

2. Especifique un nombre para la configuración de HTTP y puede habilitar o deshabilitar la Cookie de afinidad según sus necesidades.

3. Elija el protocolo como HTTP o HTTPS según el caso de uso. 

4. Active la casilla de **uso para App Service** y se activará el **cree un sondeo con el nombre de host de selección de direcciones de back-end** y **nombre de host de selección de direcciones de back-end** opciones. Esta opción también se cree un sondeo automáticamente con el modificador habilitado y asociarla a esta configuración de HTTP.

5. Haga clic en **Aceptar** para crear la configuración de HTTP.

   ![HTTP setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP setting2](./media/configure-web-app-portal/http-setting2.png)

## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Crear reglas para adaptar el agente de escucha, el grupo back-end y la configuración HTTP

1. En **reglas**, haga clic en **básica** para crear una regla básica de nuevo.

2. Proporcione un nombre adecuado y seleccione el agente de escucha que va a aceptar las solicitudes entrantes para el servicio de aplicación.

3. En el **grupo back-end** lista desplegable, elija el back-end del grupo que creó anteriormente.

4. En el **configuración HTTP** lista desplegable, elija la configuración que creó anteriormente de HTTP.

5. Haga clic en **Aceptar** para guardar esta regla.

   ![Regla](./media/configure-web-app-portal/rule.png)

## <a name="restrict-access"></a>Restricción del acceso

Las aplicaciones web implementadas en estos ejemplos usan direcciones IP públicas a las que se puede acceder directamente desde Internet. Esto ayuda a solucionar los problemas cuando se está aprendiendo una nueva característica y se prueban cosas nuevas. Pero si va a implementar una característica en un entorno de producción, tendrá que agregar más restricciones.

Una manera de restringir el acceso a las aplicaciones web es usar [Restricciones de IP estáticas de Azure App Service](../app-service/app-service-ip-restrictions.md). Por ejemplo, puede restringir la aplicación web para que solo recibe tráfico desde la puerta de enlace de aplicaciones. Use la característica de restricción de IP de servicio de aplicación para mostrar la dirección IP virtual de la puerta de enlace de aplicaciones como la única dirección con el acceso.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el servicio de aplicación y otra compatibilidad de varios inquilinos con application gateway, consulte [compatibilidad con el servicio de varios inquilinos con application gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

En caso de que la respuesta desde el servicio de aplicación redirige a la dirección URL del servicio de aplicación, consulte cómo [redirección al problema de dirección URL del servicio de aplicación de la solución de problemas](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).
