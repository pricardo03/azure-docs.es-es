---
title: Conexión de Instancia administrada de SQL de Azure para la indexación de búsqueda
titleSuffix: Azure Cognitive Search
description: Habilite el punto de conexión público para permitir conexiones con Instancias administradas de SQL desde un indexador en Búsqueda cognitiva de Azure.
manager: nitinme
author: vl8163264128
ms.author: victliu
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 0f91775e0175b4b4af9b57fa96e389c3a2a22564
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75863128"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>Configuración de una conexión desde un indexador de Búsqueda cognitiva de Azure a Instancia administrada de SQL

Como se indicó en [Conexión e indexación de contenido de Azure SQL Database mediante indexadores de Búsqueda cognitiva de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), la creación de indexadores en **Instancias administradas de SQL** es compatible con Búsqueda cognitiva de Azure a través del punto de conexión público.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Creación de una Instancia administrada de SQL de Azure con un punto de conexión público
Cree una Instancia administrada de SQL con la opción **Habilitar el punto de conexión público** seleccionada.

   ![Habilitar el punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Habilitar el punto de conexión público")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Habilitación del punto de conexión público para una Instancia administrada de SQL de Azure
También puede habilitar el punto de conexión público en una Instancia administrada de SQL existente en **Seguridad** > **Red virtual** > **Punto de conexión público**  > **Habilitar**.

   ![Habilitar el punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Habilitar el punto de conexión público")

## <a name="verify-nsg-rules"></a>Comprobación de reglas de NSG
Compruebe que el grupo de seguridad de red tiene las **reglas de seguridad de entrada** correctas que permiten las conexiones de los servicios de Azure.

   ![Regla de seguridad de entrada de NSG](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "Regla de seguridad de entrada de NSG")

> [!NOTE]
> Puede elegir ser más restrictivo en el acceso de entrada a la instancia de SQL administrada si reemplaza la regla actual (`public_endpoint_inbound`) por dos reglas:
>
> * Permitir el acceso de entrada desde la [etiqueta de servicio](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) `AzureCognitiveSearch` ("SOURCE" = `AzureCognitiveSearch`).
>
> * Permitir el acceso de entrada desde la dirección IP del servicio de búsqueda, que se puede obtener mediante un pin al nombre de dominio completo (por ejemplo, `<your-search-service-name>.search.windows.net`). ("SOURCE" = `IP address`)

## <a name="get-public-endpoint-connection-string"></a>Obtener cadena de conexión de punto de conexión público
Asegúrese de usar la cadena de conexión para el **punto de conexión público** (puerto 3342, no el puerto 1433).

   ![Cadena de conexión de punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Cadena de conexión de punto de conexión público")

## <a name="next-steps"></a>Pasos siguientes
Dejando a un lado la configuración, ya puede especificar una Instancia administrada de SQL como el origen de datos de un indexador de Búsqueda cognitiva de Azure mediante el portal o la API de REST. Consulte [Conexión de Azure SQL Database a Búsqueda cognitiva de Azure mediante indexadores](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) para más información.
