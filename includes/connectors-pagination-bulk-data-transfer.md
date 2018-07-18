---
title: archivo de inclusión
description: archivo de inclusión
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/09/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 524bc1d3a19ad8ecfc8d0d210e735d6a9ae0058b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678319"
---
### <a name="set-up-pagination"></a>Configuración de la paginación

Para algunos conectores y sus acciones que permiten recuperar varios elementos, los resultados podrían superar el tamaño de página predeterminado del conector. En este caso, la acción devolverá solo la primera página de resultados. Por ejemplo, el tamaño de página predeterminado para la acción **SQL Server - Get rows** es 2048, pero puede ser diferente según la configuración. Para asegurarse de que se obtienen todos los registros, active la opción **Paginación** para esa acción. Esta opción hace que la aplicación lógica pida al conector los registros restantes, pero devuelve todos los resultados como un único mensaje cuando finaliza la acción. 

Estos son solo algunos conectores donde puede activar la paginación para acciones concretas: 

* <a href="https://docs.microsoft.com/connectors/db2/" target="_blank">DB2</a>
* <a href="https://docs.microsoft.com/connectors/dynamicscrmonline/" target="_blank">Dynamics 365 CRM Online</a>
* <a href="https://docs.microsoft.com/connectors/excel/" target="_blank">Excel</a>
* <a href="https://docs.microsoft.com/connectors/oracle/" target="_blank">Oracle Database</a>
* <a href="https://docs.microsoft.com/connectors/sharepointonline/" target="_blank">SharePoint</a>
* <a href="https://docs.microsoft.com/connectors/sql/" target="_blank">SQL Server</a> 

Este es un ejemplo de la acción **Get rows**:

1. Para averiguar si la acción admite la paginación, abra la **configuración** de la acción. 

   ![En la acción, abra "Configuración".](./media/connectors-pagination-bulk-data-transfer/sql-action-settings.png)

2. Si la acción admite la paginación, cambie la configuración de **Paginación** de **Desactivada** a **Activada**. Para asegurarse de que la acción devuelve un conjunto mínimo de resultados, especifique un valor como **límite**.

   ![Especifique que la acción devuelva un número mínimo de resultados](./media/connectors-pagination-bulk-data-transfer/sql-action-settings-pagination.png)

3. Cuando esté listo, elija **Hecho**.