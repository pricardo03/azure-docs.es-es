---
title: Actualización con aplicaciones lógicas para los modelos de Azure Analysis Services | Microsoft Docs
description: Obtenga información sobre cómo programar la actualización asincrónica mediante el uso de Azure Logic Apps.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 6e1ac5dfd1972e406a1bd8dcd26e6aef2c4ea6d1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919864"
---
# <a name="refresh-with-logic-apps"></a>Actualizar con Logic Apps

Mediante el uso de Logic Apps y llamadas de REST, puede realizar operaciones de actualización de datos automatizada en los modelos tabulares de análisis de Azure, incluida la sincronización de réplicas de solo lectura para el escalado horizontal de la consulta.

Para más información sobre cómo usar las API de REST con Azure Analysis Services, consulte [asincrónico de actualización con la API de REST](analysis-services-async-refresh.md).

## <a name="authentication"></a>Authentication

Todas las llamadas deben autenticarse con un token válido de Azure Active Directory (OAuth 2).  Los ejemplos de este artículo usará un Principal de servicio (SPN) para autenticarse en Azure Analysis Services. Para obtener más información, consulte [crear una entidad de servicio mediante Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="design-the-logic-app"></a>Diseñar la aplicación lógica

> [!IMPORTANT]
> Los ejemplos siguientes se supone que el firewall de Azure Analysis Services está deshabilitado.  Si el firewall está habilitado, la dirección IP pública del iniciador de la solicitud debe ser la lista de permitidos en firewall de Azure Analysis Services. Para más información acerca de los intervalos de IP de la aplicación lógica por región, consulte [límites y la información de configuración para Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses).

### <a name="prerequisites"></a>Requisitos previos

#### <a name="create-a-service-principal-spn"></a>Crear a una entidad de servicio (SPN)

Para obtener información acerca de cómo crear una entidad de servicio, consulte [crear una entidad de servicio mediante Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="configure-permissions-in-azure-analysis-services"></a>Configurar permisos en Azure Analysis Services
 
La entidad de servicio que cree debe tener permisos de administrador del servidor en el servidor. Para más información, vea [Incorporación de una entidad de servicio al rol de administrador del servidor](analysis-services-addservprinc-admins.md).

### <a name="configure-the-logic-app"></a>Configurar la aplicación lógica

En este ejemplo, la aplicación lógica está diseñada para desencadenar cuando se recibe una solicitud HTTP. Esto permitirá el uso de una herramienta de orquestación, como Azure Data Factory, para desencadenar la actualización del modelo de Azure Analysis Services.

Una vez haya creado una aplicación lógica:

1. En el Diseñador de aplicación lógica, elija la primera acción como **cuando recibe una solicitud HTTP es**.

   ![Agregar actividad HTTP recibido](./media/analysis-services-async-refresh-logic-app/1.png)

Este paso se rellena con la dirección URL de POST de HTTP una vez que se guarda la aplicación lógica.

2. Agregar un nuevo paso y busque **HTTP**.  

   ![Agregar actividad HTTP](./media/analysis-services-async-refresh-logic-app/9.png)

   ![Agregar actividad HTTP](./media/analysis-services-async-refresh-logic-app/10.png)

3. Seleccione **HTTP** para agregar esta acción.

   ![Agregar actividad HTTP](./media/analysis-services-async-refresh-logic-app/2.png)

Configurar la actividad HTTP como sigue:

|Propiedad  |Valor  |
|---------|---------|
|**Método**     |POST         |
|**URI**     | https://*su región de servidor*/Servers /*nombre del servidor aas*/models/*su nombre de la base de datos*/ <br /> <br /> Por ejemplo: https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/|
|**Encabezados**     |   Content-Type, application/json <br /> <br />  ![encabezados](./media/analysis-services-async-refresh-logic-app/6.png)    |
|**Cuerpo**     |   Para obtener más información acerca de que forman el cuerpo de solicitud, vea [asincrónico de actualización con la API de REST, / refreshes POST](analysis-services-async-refresh.md#post-refreshes). |
|**Autenticación**     |Active Directory OAuth         |
|**Tenant**     |Rellene el valor de TenantId de Azure Active Directory         |
|**Audiencia**     |https://*.asazure.windows.net         |
|**Id. de cliente**     |Especifique el ClientID de nombre de entidad de servicio         |
|**Tipo de credencial**     |Secret         |
|**Secreto**     |Escriba el secreto del nombre de entidad de servicio         |

Ejemplo:

![Actividad completada de HTTP](./media/analysis-services-async-refresh-logic-app/7.png)

Probar ahora la aplicación lógica.  En el Diseñador de aplicación lógica, haga clic en **ejecutar**.

![Probar la aplicación lógica](./media/analysis-services-async-refresh-logic-app/8.png)

## <a name="consume-the-logic-app-with-azure-data-factory"></a>Utilizar la aplicación lógica con Azure Data Factory

Una vez que se guarda la aplicación lógica, revise el **cuando recibe una solicitud HTTP es** actividad y a continuación, copie el **URL de HTTP POST** que ahora se genera.  Se trata de la dirección URL que puede usarse con Azure Data Factory para realizar la llamada asincrónica a la aplicación lógica del desencadenador.

Este es un ejemplo de actividad Azure Data Factory Web que realiza esta acción.

![Actividad Web de Data Factory](./media/analysis-services-async-refresh-logic-app/11.png)

## <a name="use-a-self-contained-logic-app"></a>Usar una aplicación lógica independiente

Si no tiene pensado sobre el uso de una herramienta de orquestación como Data Factory para desencadenar la actualización del modelo, puede establecer la aplicación lógica para desencadenar la actualización según una programación.

Mediante el ejemplo anterior, elimine la primera actividad y reemplazarlo con un **programación** actividad.

![Actividad de programación](./media/analysis-services-async-refresh-logic-app/12.png)

![Actividad de programación](./media/analysis-services-async-refresh-logic-app/13.png)

Este ejemplo usaremos **periodicidad**.

Una vez que se ha agregado la actividad, configurar el intervalo y la frecuencia, a continuación, agregue un nuevo parámetro y elija **a estas horas**.

![Actividad de programación](./media/analysis-services-async-refresh-logic-app/16.png)

Seleccione las horas deseadas.

![Actividad de programación](./media/analysis-services-async-refresh-logic-app/15.png)

Guarde la aplicación lógica.

## <a name="next-steps"></a>Pasos siguientes

[Muestras](analysis-services-samples.md)  
[API DE REST](https://docs.microsoft.com/rest/api/analysisservices/servers)
