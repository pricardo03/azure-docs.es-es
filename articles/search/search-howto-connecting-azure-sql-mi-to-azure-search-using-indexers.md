---
title: 'Conexión de Instancia administrada de SQL de Azure para la indexación de búsqueda: Azure Search'
description: Habilite el punto de conexión público para permitir conexiones con Instancias administradas de SQL desde un indexador en Azure Search.
author: vl8163264128
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: victliu
ms.openlocfilehash: 07bc1a55c0222fda87b28acbaa8bfe552fb8e6ed
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186609"
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-managed-instance"></a>Configuración de una conexión desde un indexador de Azure Search a Instancia administrada de SQL
Como se indicó en [Conexión e indexación de contenido de Azure SQL Database mediante indexadores de Azure Search](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), la creación de indexadores en **Instancias administradas de SQL** es compatible con Azure Search a través del punto de conexión público.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Creación de una Instancia administrada de SQL de Azure con un punto de conexión público
Cree una Instancia administrada de SQL con la opción **Habilitar el punto de conexión público** seleccionada.

   ![Habilitar el punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Enable public endpoint")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Habilitación del punto de conexión público para una Instancia administrada de SQL de Azure
También puede habilitar el punto de conexión público en una Instancia administrada de SQL existente en **Seguridad** > **Red virtual** > **Punto de conexión público**  > **Habilitar**.

   ![Habilitar el punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Enable public endpoint")

## <a name="verify-nsg-rules"></a>Comprobación de reglas de NSG
Compruebe que el grupo de seguridad de red tiene las **reglas de seguridad de entrada** correctas que permiten las conexiones de los servicios de Azure.

   ![Regla de seguridad de entrada de NSG](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG Inbound security rule")

## <a name="get-public-endpoint-connection-string"></a>Obtener cadena de conexión de punto de conexión público
Asegúrese de usar la cadena de conexión para el **punto de conexión público** (puerto 3342, no el puerto 1433).

   ![Cadena de conexión de punto de conexión público](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Public endpoint connection string")

## <a name="next-steps"></a>Pasos siguientes
Dejando a un lado la configuración, ya puede especificar una Instancia administrada de SQL como el origen de datos de un indexador de Azure Search mediante el portal o la API REST. Consulte [Conexión de Azure SQL Database a Azure Search mediante indexadores](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) para más información.
