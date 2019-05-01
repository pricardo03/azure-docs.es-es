---
title: Obtener más elementos, datos o registros con la paginación - Azure Logic Apps
description: Configurar la paginación para superar el límite de tamaño de página predeterminado para las acciones de conector en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 2d1bcf2cf83fab106f79120c3caacc424f839836
ms.sourcegitcommit: daf6538427ea6effef898f2ee3d857e5fa2dccbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64476547"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Obtener más datos, los elementos o registros mediante la paginación en Azure Logic Apps

Al recuperar datos, los elementos o registros mediante el uso de una acción del conector en [Azure Logic Apps](../logic-apps/logic-apps-overview.md), podría obtener conjuntos de resultados tan grande que la acción no devuelve todos los resultados al mismo tiempo. Con algunas acciones, el número de resultados puede superar el tamaño de página predeterminado del conector. En este caso, la acción devolverá solo la primera página de resultados. Por ejemplo, el tamaño de página predeterminado para el conector de SQL Server **obtener filas** acción es 2048, pero puede variar según otros valores de configuración.

Algunas acciones permiten activar un *paginación* establecer para que la aplicación lógica puede recuperar más resultados hasta el límite de paginación, pero devuelve los resultados como un mensaje único cuando finaliza la acción. Cuando se utiliza la paginación, debe especificar un *umbral* valor, que es el número de destino de resultados que desea que la acción a devolver. La acción recupera los resultados hasta alcanzar el umbral especificado. Cuando el número total de elementos es menor que el umbral especificado, la acción recupera todos los resultados.

Activar las páginas de recupera de configuración de la paginación de resultados según el tamaño de página de un conector. Este comportamiento significa que, en ocasiones, podría obtener resultados más que el umbral especificado. Por ejemplo, al usar SQL Server **obtener filas** acción, que admite la paginación:

* Tamaño de página predeterminado de la acción es 2048 registros por página.
* Suponga que tiene 10.000 registros y especifica 5000 registros como mínimo.
* Paginación Obtiene las páginas de registros, por lo que para obtener al menos el mínimo especificado, la acción devuelve 6144 registros a (3 páginas x 2048), no 5000 registros.

Esta es una lista con sólo algunos de los conectores que puede superar el tamaño de página predeterminado para acciones específicas:

* [Azure Blob Storage](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Oracle Database](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La aplicación lógica y la acción que desea activar la paginación. Si no tiene una aplicación lógica, consulte [inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Activar la paginación

Para determinar si una acción admite la paginación en el Diseñador de aplicaciones lógicas, compruebe la configuración de la acción para el **paginación** configuración. En este ejemplo se muestra cómo activar la paginación en el servidor de SQL Server **obtener filas** acción.

1. En la esquina superior derecha de la acción, elija el botón de puntos suspensivos (**...** ) y seleccione **configuración**.

   ![Abra la configuración de la acción](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Si la acción es compatible con la paginación, la acción se muestra el **paginación** configuración.

1. Cambiar el **paginación** de **desactivar** a **en**. En el **umbral** propiedad, especifique un valor entero para el número de destino de resultados que desea que la acción a devolver.

   ![Especifique el número mínimo de resultados para devolver](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Cuando esté listo, elija **Hecho**.

## <a name="workflow-definition---pagination"></a>Definición de flujo de trabajo: paginación

Al activar la paginación de una acción que admite esta funcionalidad, la definición de flujo de trabajo de la aplicación lógica incluye el `"paginationPolicy"` propiedad junto con la `"minimumItemCount"` propiedad en esa acción `"runtimeConfiguration"` propiedad, por ejemplo:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
