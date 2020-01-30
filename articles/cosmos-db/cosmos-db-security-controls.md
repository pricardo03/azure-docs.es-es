---
title: Controles de seguridad para Azure Cosmos DB
description: Obtenga una lista de comprobación de los controles de seguridad como la red, la supervisión, la identidad y la protección de datos para evaluar Azure Cosmos DB
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.openlocfilehash: d06afb3670b1c3af3f1acf0bd690c1e3e266a11c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76771785"
---
# <a name="security-controls-for-azure-cosmos-db"></a>Controles de seguridad para Azure Cosmos DB

En este artículo, se explican los controles de seguridad integrados en Azure Cosmos DB.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/no | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sí | Con el punto de conexión de servicio de red virtual, puede configurar la cuenta de Azure Cosmos DB para permitir el acceso solo desde una subred específica de una red virtual (VNet). También puede combinar el acceso de red virtual con reglas de firewall. Para más información, consulte [Acceso a Azure Cosmos DB desde redes virtuales](VNet-service-endpoint.md). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Gracias a la compatibilidad con el firewall, puede configurar su cuenta de Azure Cosmos para permitir el acceso solo desde un conjunto de direcciones IP aprobado, un intervalo de direcciones IP o servicios en la nube. Para más información, consulte [Configuración del firewall de IP en Azure Cosmos DB](how-to-configure-firewall.md).|
| Compatibilidad con la tunelización forzada| Sí | Se puede configurar en el cliente en la red virtual donde se encuentran las máquinas virtuales.   |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/no | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Se registran todas las solicitudes que se envían a Azure Cosmos DB. Se admiten la [supervisión de Azure](../azure-monitor/overview.md), las métricas de Azure y el registro de auditoría de Azure.  Puede registrar la información correspondiente a las solicitudes de plano de datos, las estadísticas de tiempo de ejecución de consultas, el texto de la consulta y las solicitudes de MongoDB. También puede configurar alertas. |
| Registro y auditoría del plano de administración y de control| Sí | El registro de actividad de Azure para las operaciones de nivel de cuenta, como Firewalls, redes virtuales, acceso con claves e IAM. |
| Registro y auditoría del plano de datos | Sí | Registro de supervisión de diagnósticos para las operaciones de nivel de contenedor, como la creación contenedores, el aprovisionamiento de rendimiento, la indexación de directivas y las operaciones CRUD en documentos. |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/no | Notas|
|---|---|--|
| Authentication| Sí | Sí en el nivel de cuenta de base de datos; en el nivel del plano de datos, Cosmos DB usa tokens de recursos y acceso con clave. |
| Authorization| Sí | Se admite en la cuenta de Azure Cosmos con las claves maestras (principal y secundaria) y tokens de recursos. Puede obtener acceso de lectura/escritura o acceso de solo lectura a los datos con las claves maestras. Los tokens de recursos permiten el acceso de tiempo limitado a los recursos, como documentos y contenedores. |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/no | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Todas las bases de datos y las copias de seguridad de Azure Cosmos se cifran de forma predeterminada; consulte [Cifrado de datos en Azure Cosmos DB](database-encryption-at-rest.md). |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | Vea [Configuración de las claves administradas por el cliente para la cuenta de Azure Cosmos DB](how-to-setup-cmk.md)  |
| Cifrado de nivel de columna (Azure Data Services)| Sí | Solo en las instancias de Table API Premium. No todas las API admiten esta característica. Consulte [Introducción a Azure Cosmos DB: Table API](table-introduction.md). |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Todos los datos de Azure Cosmos DB se cifran en tránsito. |
| Llamadas a API cifradas| Sí | Todas las conexiones a Azure Cosmos DB admiten HTTPS. Azure Cosmos DB también admite TLS 1.2.<br>Es posible aplicar una versión de TLS mínima en el servidor. Para ello, póngase en contacto con [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com). |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| No  | | 

## <a name="additional-security-controls-for-cosmos-db"></a>Controles de seguridad adicionales para Cosmos DB

| Control de seguridad | Sí/no | Notas|
|---|---|--|
| Uso compartido de recursos entre orígenes (CORS) | Sí | Consulte [Configuración del uso compartido de recursos entre orígenes (CORS)](how-to-configure-cross-origin-resource-sharing.md). |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).