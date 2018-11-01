---
title: Eliminación de datos desde el Explorador de datos de Azure
description: En este artículo se describen los escenarios de eliminación masiva en el Explorador de datos de Azure, incluidas las eliminaciones basadas en la retención y la purga.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 485ee48bf197ad996bba8a516b80882f44ba623a
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212920"
---
# <a name="delete-data-from-azure-data-explorer"></a>Eliminación de datos desde el Explorador de datos de Azure

El Explorador de datos de Azure admite varias soluciones de eliminación masiva, que trataremos en este artículo. No admite la eliminación por registro en tiempo real, porque está optimizado para agilizar el acceso de lectura.

* Si una o varias tablas ya no se necesitan, elimínelas mediante los comandos drop table o drop tables.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Si los datos antiguos ya no se necesitan, elimínelos; para ello, cambie el período de retención en el nivel de base de datos o de tabla.

    Considere una base de datos o una tabla que se establece con 90 días de retención. Las necesidades empresariales cambian, de modo que solo es necesario mantener los datos 60 días. En este caso, elimine los datos más antiguos de una de las maneras siguientes.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Para más información, consulte [Directiva de retención](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

Si necesita ayuda con problemas relacionados con la eliminación de datos, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
