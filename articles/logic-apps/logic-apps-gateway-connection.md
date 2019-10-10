---
title: Acceso a orígenes de datos locales desde Azure Logic Apps
description: Conexión a orígenes de datos locales desde aplicaciones lógicas mediante la creación de una puerta de enlace de datos local
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 09/01/2019
ms.openlocfilehash: d397adfb6ed2d3aef93bd40d14eb0ad199cdb90c
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309353"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Conexión a orígenes de datos locales desde Azure Logic Apps

Para acceder a orígenes de datos locales desde las aplicaciones lógicas, cree un recurso de puerta de enlace de datos local en Azure Portal. Las aplicaciones lógicas pueden usar entonces los [conectores locales](../connectors/apis-list.md#on-premises-connectors). Azure Logic Apps admite operaciones de escritura, incluidas inserciones y actualizaciones, mediante la puerta de enlace. Sin embargo, estas operaciones tienen [límites en su tamaño de carga](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations).

En este artículo se muestra cómo crear el recurso de puerta de enlace de Azure *después* de [descargar e instalar la puerta de enlace en un equipo local](../logic-apps/logic-apps-gateway-install.md). Para más información sobre la puerta de enlace, consulte el [funcionamiento de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service). 

> [!TIP]
> Para conectarse a redes virtuales de Azure, considere la posibilidad de crear en su lugar un [*entorno del servicio de integración*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). 

Para información sobre cómo usar la puerta de enlace con otros servicios, consulte estos artículos:

* [Puerta de enlace de datos local de Microsoft Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Puerta de enlace de datos local de Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Administración de una puerta de enlace de datos local en Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Puerta de enlace de datos local de Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

<a name="supported-connections"></a>

## <a name="supported-data-sources"></a>Orígenes de datos admitidos

En Azure Logic Apps, la puerta de enlace de datos local admite [conectores locales](../connectors/apis-list.md#on-premises-connectors) para estos orígenes de datos:

* BizTalk Server 2016
* Sistema de archivos
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Base de datos de Oracle
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Aunque la puerta de enlace por sí sola no incurre en costos adicionales, el [modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md) se aplica a estos conectores y a otras operaciones en Azure Logic Apps.

## <a name="prerequisites"></a>Requisitos previos

* Ya ha [instalado la puerta de enlace de datos local en un equipo local](../logic-apps/logic-apps-gateway-install.md).

* Tiene la [misma cuenta de Azure y suscripción de Azure](../logic-apps/logic-apps-gateway-install.md#requirements) que usó al instalar la puerta de enlace de datos local.

* No ha vinculado previamente la instalación de la puerta de enlace a otro recurso de puerta de enlace en Azure.

  Al crear un recurso de puerta de enlace, selecciona una instalación de puerta de enlace para asociarla con el recurso de puerta de enlace. Una instalación de puerta de enlace ya vinculada no está disponible para su selección al crear recursos de puerta de enlace.

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Creación de un recurso de puerta de enlace de Azure

Después de instalar la puerta de enlace en un equipo local, cree un recurso de Azure para la puerta de enlace. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con la misma cuenta de Azure que se usó para instalar la puerta de enlace.

1. En el cuadro de búsqueda de Azure Portal, escriba "puerta de enlace de datos local" y luego elija **Puertas de enlace de datos locales**.

   ![Busque "Puerta de enlace de datos local".](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

1. Busque **Puertas de enlace de datos locales** y seleccione **Agregar**.

   ![Adición de puerta de enlace de datos](./media/logic-apps-gateway-connection/add-gateway.png)

1. En **Crear puerta de enlace de conexión**, especifique la siguiente información el recurso de puerta de enlace. Seleccione **Crear** cuando haya terminado.

   | Propiedad | DESCRIPCIÓN |
   |----------|-------------|
   | **Nombre de recurso** | El nombre del recurso de puerta de enlace, que solo puede contener letras, números, guiones (`-`), caracteres de subrayado (`_`), paréntesis (`(`,`)`) y puntos (`.`). |
   | **Suscripción** | Su suscripción de Azure, que debe ser la misma que la de la instalación de puerta de enlace y la aplicación lógica. La suscripción predeterminada se basa en la cuenta de Azure que usó para iniciar sesión. |
   | **Grupos de recursos** | El [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) que quiere usar. |
   | **Ubicación** | La misma región que la ubicación seleccionada para el servicio en la nube de la puerta de enlace durante la [instalación de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md). De lo contrario, la instalación de la puerta de enlace no aparecerá en la lista **Nombre de la instalación** para que la pueda seleccionar. La ubicación de la aplicación lógica puede diferir de la ubicación del recurso de la puerta de enlace. |
   | **Nombre de instalación** | Si la instalación de la puerta de enlace no está seleccionada, seleccione la puerta de enlace que instaló anteriormente. Las instalaciones de puerta de enlace vinculadas anteriormente no aparecerán en esta lista y, por tanto, no podrá seleccionarlas. |
   |||

   Este es un ejemplo:

   ![Proporcionar detalles para crear la puerta de enlace de datos local](./media/logic-apps-gateway-connection/gateway-details.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Conectarse a datos locales

Después de crear el recurso de puerta de enlace y asociar su suscripción de Azure con este recurso, ahora puede crear una conexión entre la aplicación lógica y el origen de datos local mediante el uso de la puerta de enlace.

1. En Azure Portal, cree o abra la aplicación lógica en el Diseñador de aplicación lógica.

1. Agregue un conector que admita conexiones locales como, por ejemplo, **SQL Server**.

1. Seleccione **Connect via on-premises data gateway** (Conectarse a través de la puerta de enlace de datos local). 

1. En **Puertas de enlace**, seleccione el recurso de puerta de enlace que ha creado.

   > [!NOTE]
   > La lista de puertas de enlace incluye recursos de puerta de enlace de otras regiones porque la ubicación de la aplicación lógica puede diferir de la ubicación del recurso de la puerta de enlace.

1. Proporcione un nombre de conexión único y otra información necesaria, que depende de la conexión que desee crear.

   Un nombre de conexión único le ayuda a identificar fácilmente la conexión más adelante, especialmente al crear varias conexiones. Si procede, incluya también el dominio completo para el nombre de usuario.
   
   Este es un ejemplo:

   ![Creación de conexiones entre la aplicación lógica y la puerta de enlace de datos](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. Seleccione **Crear** cuando haya terminado. 

La conexión de puerta de enlace ya está lista para que la use la aplicación lógica.

## <a name="edit-connection"></a>Edición de la conexión

Para actualizar la configuración de una conexión de puerta de enlace, puede editar la conexión.

1. Para encontrar todas las conexiones de API únicamente para su aplicación lógica, en el menú de la aplicación lógica, seleccione **Conexiones de API** en **Herramientas de desarrollo**.
   
   ![En el menú de la aplicación lógica, seleccione "Conexiones de API".](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

1. Seleccione la conexión de puerta de enlace que desee y elija **Editar la conexión de API**.

   > [!TIP]
   > Si las actualizaciones no surten efecto, intente [detener y reiniciar la cuenta de servicio de Windows de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md#restart-gateway) para su instalación de puerta de enlace.

Para encontrar todas las conexiones de API asociadas a su suscripción de Azure: 

* En el menú principal de Azure, vaya a **Todos los servicios** > **Web** > **Conexiones de API**.
* O bien, vaya a **Todos los recursos** en el menú principal de Azure. Establezca el filtro **Tipo** en **Conexión de API**.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Eliminación del recurso de puerta de enlace

Para crear un recurso de puerta de enlace diferente, vincular la instalación de la puerta de enlace a un recurso de puerta de enlace diferente o quitar el recurso de puerta de enlace, puede eliminar el recurso de puerta de enlace sin que afecte a la instalación de la puerta de enlace. 

1. En el menú principal de Azure, seleccione **Todos los recursos**. Busque y seleccione el recurso de puerta de enlace.

1. Si aún no está seleccionada, en el menú de recursos de puerta de enlace, seleccione **Puerta de enlace de datos local**. En la barra de herramientas de recursos de puerta de enlace, seleccione **Eliminar**.

   Por ejemplo:

   ![Eliminar puerta de enlace](./media/logic-apps-gateway-connection/gateway-delete.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P.** : ¿Por qué no puedo ver mi instalación de puerta de enlace al crear el recurso de puerta de enlace en Azure? <br/>
**R.** : Este problema puede deberse a estos motivos:

* La instalación de puerta de enlace está registrada y ha sido reclamada por otro recurso de puerta de enlace en Azure. Las instalaciones de puerta de enlace no aparecen en la lista de instancias después de crear los recursos de puerta de enlace para ellas. Para comprobar los registros de puerta de enlace en Azure Portal, revise todos los recursos de Azure de tipo **Puertas de enlace de datos local** para *todas* las suscripciones de Azure.

* La identidad de Azure AD para la persona que instaló la puerta de enlace difiere de la persona que inició sesión en Azure Portal. Compruebe que ha iniciado sesión con la misma identidad que instaló la puerta de enlace.

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Protección de las aplicaciones lógicas](./logic-apps-securing-a-logic-app.md)
* [Ejemplos y escenarios habituales de las aplicaciones lógicas](./logic-apps-examples-and-scenarios.md)
