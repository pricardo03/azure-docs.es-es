---
title: Conectividad SSL de Azure Database for MariaDB
description: Información para configurar Azure Database for MariaDB y las aplicaciones asociadas, a fin de usar correctamente las conexiones SSL
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 15bb28846b3409dd31bcdf8d42990facc94fd06d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332693"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>Conectividad SSL en Azure Database for MariaDB
Azure Database for MariaDB permite conectar el servidor de bases de datos a las aplicaciones cliente con la Capa de sockets seguros (SSL). Aplicar conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

## <a name="default-settings"></a>Configuración predeterminada
De forma predeterminada, el servicio de base de datos debe configurarse para requerir conexiones SSL al conectar con MariaDB.  Se recomienda evitar la desactivación de la opción SSL siempre que sea posible.

Al aprovisionar un nuevo servidor de Azure Database for MariaDB en Azure Portal o con la CLI de Azure, el establecimiento de conexiones SSL está habilitado de forma predeterminada.

Las cadenas de conexión para distintos lenguajes de programación se muestran en Azure Portal. Estas cadenas de conexión incluyen los parámetros SSL que se requieren para conectarse a la base de datos. En Azure Portal, seleccione el servidor. En el encabezado **Configuración**, seleccione las **cadenas de conexión**. El parámetro SSL varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

Para información sobre cómo habilitar o deshabilitar la conexión SSL al desarrollar la aplicación, consulte [Configuración de SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [las reglas de firewall del servidor](concepts-firewall-rules.md)
- Obtenga más información sobre cómo [configurar SSL](howto-configure-ssl.md).
