---
title: 'Visualización de un almacén de conocimiento con el Explorador de Storage: Azure Search'
description: Visualice y analice un almacén de conocimiento de Azure Search con el Explorador de Storage de Azure Portal.
author: lisaleib
services: search
ms.service: search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: 9ec93bcd2309bb47b24983260c35726ac24b5e95
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265613"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Visualización de un almacén de conocimiento con el Explorador de Storage

> [!Note]
> El almacén de conocimiento se encuentra en versión preliminar y no debe usarse en producción. Esta característica se proporciona en la [API REST de Azure Search, versión 2019-05-06-Preview](search-api-preview.md). Por el momento, no hay compatibilidad con .NET SDK.
>
En este artículo, aprenderá a conectar y explorar un almacén de conocimiento con el Explorador de Storage en Azure Portal. Para crear el ejemplo de almacén de conocimiento que se usa en este tutorial, consulte [Creación de un almacén de conocimiento en Azure Portal](knowledge-store-create-portal.md).

## <a name="prerequisites"></a>Requisitos previos

+ Siga los pasos descritos en [Creación de un almacén de conocimiento en Azure Portal](knowledge-store-create-portal.md) para crear el almacén de conocimiento de ejemplo que se usa en este tutorial.

+ También necesitará el nombre de la cuenta de Azure Storage que usó para crear el almacén de conocimiento, junto con su clave de acceso de Azure Portal.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Ver, editar y consultar un almacén de conocimiento en el Explorador de Storage

1. En Azure Portal, abra la cuenta de almacenamiento que usó para crear el almacén de conocimiento.

1. En el panel de navegación izquierdo de la cuenta de almacenamiento, haga clic en **Explorador de Storage**.

1. Expanda la lista **Tablas** para mostrar una lista de las proyecciones de tablas de Azure que se crearon al ejecutar el asistente para **Importar datos** en los datos de ejemplo de reseñas de hoteles.

Seleccione una tabla para ver los datos enriquecidos, incluidas las puntuaciones de opinión de las frases clave, los datos de latitud y longitud de la ubicación, etc.

   ![Ver tablas en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Ver tablas en el Explorador de Storage")

Para cambiar el tipo de datos de cualquier valor de tabla o para cambiar los valores individuales de la tabla, haga clic en **Editar**. Al cambiar el tipo de datos de una columna de una fila de la tabla, se aplicará a todas las filas.

   ![Editar tabla en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Editar tabla en el Explorador de Storage")

Para ejecutar consultas, haga clic en **Consultar** en la barra de comandos y escriba las condiciones.  

   ![Consultar tabla en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Consultar tabla en el Explorador de Storage")

## <a name="clean-up"></a>Limpieza

Cuando trabaje con su propia suscripción, es una buena idea al final de un proyecto identificar si todavía se necesitan los recursos que ha creado. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Puede eliminar los recursos de forma individual o eliminar el grupo de recursos para eliminar todo el conjunto de recursos.

Puede encontrar y administrar recursos en el portal, mediante el vínculo **Todos los recursos** o **Grupos de recursos** en el panel de navegación izquierdo.

Si está usando un servicio gratuito, recuerde que está limitado a tres índices, indexadores y orígenes de datos. Puede eliminar elementos individuales en el portal para mantenerse por debajo del límite.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo conectar este almacén de conocimiento a Power BI, consulte el siguiente artículo.

> [!div class="nextstepaction"]
> [Conexión con Power BI](knowledge-store-connect-power-bi.md)

Para obtener información sobre cómo crear un almacén de conocimiento mediante las API REST y Postman, consulte el artículo siguiente.  

> [!div class="nextstepaction"]
> [Creación de un almacén de conocimiento con REST](knowledge-store-howto.md)
