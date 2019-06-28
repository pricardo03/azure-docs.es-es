---
title: Administración del tráfico a aplicaciones multiinquilino, como aplicaciones web de App Service con Azure Application Gateway - Portal
description: En este artículo se proporciona una guía sobre cómo configurar aplicaciones web de servicio de App de Azure como miembros de un grupo de back-end en una puerta de enlace de aplicaciones nueva o existente.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/11/2019
ms.author: absha
ms.openlocfilehash: 4dae04c14f9132c54dcc0575ccb2841a4742a626
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831220"
---
# <a name="configure-app-service-with-application-gateway"></a>Configuración de App Service con Application Gateway

Application Gateway permite tener una aplicación web del servicio de App de Azure u otros servicios multiinquilino como miembro del grupo de back-end. 

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> - Crear un grupo de back-end y agregarle un App Service.
> - Crear una configuración HTTP y un sondeo personalizado con los modificadores "Seleccionar nombre de host" habilitados.

## <a name="prerequisites"></a>Requisitos previos

- Puerta de enlace de aplicaciones: Si no se dispone de una puerta de enlace de aplicaciones, consulte cómo [crear una puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).
- Servicio de aplicaciones: Si no se tiene un servicio de aplicaciones existente, consulte la [documentación de servicio de aplicaciones](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Agregación de servicio de aplicaciones como grupo de back-end

1. En Azure Portal, abra la vista de configuración de la puerta de enlace de aplicaciones.

2. En **Grupos de back-end**, haga clic en **Agregar** para crear un nuevo grupo de back-end.

3. Proporcione un nombre adecuado para el grupo de back-end. 

4. En **Destinos**, haga clic en la lista desplegable y elija **App Services** como opción.

5. Aparecerá una lista desplegable con los App Services justo debajo de la lista desplegable **Destinos**. En esta lista desplegable, elija el App Service que quiera agregar como miembro del grupo de back-end y haga clic en Agregar.

   ![Back-end del servicio de aplicaciones](./media/configure-web-app-portal/backendpool.png)

## <a name="create-http-settings-for-app-service"></a>Creación de una configuración HTTP para el servicio de aplicaciones

1. En **Configuración HTTP**, haga clic en **Agregar** para crear una nueva configuración HTTP.

2. Especifique un nombre para la configuración HTTP y podrá habilitar o deshabilitar la Afinidad basada en cookies según sus requisitos.

3. Elija el protocolo como HTTP o HTTPS según el caso de uso. 

4. Marque la casilla de **Uso para App Service** y se activarán las opciones **Crear un sondeo con un nombre de host de selección de la dirección de back-end** y **Seleccionar el nombre de host de la dirección de back-end**. Esta opción también creará un sondeo de forma automática con el modificador habilitado y lo asociará a esta configuración HTTP.

5. Haga clic en **Aceptar** para crear la configuración HTTP.

   ![HTTP-setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-setting2](./media/configure-web-app-portal/http-setting2.png)

## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Creación de reglas para enlazar el cliente de escucha, el grupo de back-end y la configuración HTTP

1. En **Reglas**, haga clic en **Básicas** para crear una regla básica nueva.

2. Proporcione un nombre adecuado y seleccione el cliente de escucha que va a aceptar las solicitudes entrantes para el servicio de aplicaciones.

3. En la lista desplegable **Grupo de back-end**, elija el grupo de back-end que se ha creado anteriormente.

4. En la lista desplegable **Configuración HTTP**, elija la configuración HTTP que se ha creado anteriormente.

5. Haga clic en **Aceptar** para guardar esta regla.

   ![Regla](./media/configure-web-app-portal/rule.png)

## <a name="restrict-access"></a>Restricción del acceso

Las aplicaciones web implementadas en estos ejemplos usan direcciones IP públicas a las que se puede acceder directamente desde Internet. Esto ayuda a solucionar los problemas cuando se está aprendiendo una nueva característica y se prueban cosas nuevas. Pero si va a implementar una característica en un entorno de producción, tendrá que agregar más restricciones.

Una manera de restringir el acceso a las aplicaciones web es usar [Restricciones de IP estáticas de Azure App Service](../app-service/app-service-ip-restrictions.md). Por ejemplo, puede restringir la aplicación web para que solo recibe tráfico desde la puerta de enlace de aplicaciones. Use la característica de restricción de IP de servicio de aplicación para mostrar la dirección IP virtual de la puerta de enlace de aplicaciones como la única dirección con el acceso.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el servicio de aplicaciones y otras compatibilidades multiinquilino con la puerta de enlace de aplicaciones, consulte la [compatibilidad del servicio multiinquilino con la puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

En caso de que la respuesta del servicio de aplicaciones se redireccione a la URL del servicio de aplicaciones, consulte cómo [solucionar problemas de redireccionamiento a la dirección URL del servicio de aplicaciones](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).
