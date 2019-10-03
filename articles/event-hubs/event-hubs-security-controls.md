---
title: Controles de seguridad para Azure Event Hubs
description: Lista de comprobación de los controles de seguridad para evaluar Azure Event Hubs
services: event-hubs
ms.service: event-hubs
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: 277a745d26961ed509258d5423fc3c0da9b79a24
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219398"
---
# <a name="security-controls-for-azure-event-hubs"></a>Controles de seguridad para Azure Event Hubs

En este artículo se explican los controles de seguridad integrados en Azure Event Hubs.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |  |
| Compatibilidad con la inserción de redes virtuales| Sin | |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |  |
| Compatibilidad con la tunelización forzada| Sin |  |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |  |
| Registro y auditoría del plano de administración y de control| Sí |  |  |
| Registro y auditoría del plano de datos| Sí |   |  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Authentication| Sí | | [Autorización del acceso a Azure Event Hubs](authorize-access-event-hubs.md), [Autorización del acceso a recursos de Event Hubs mediante Azure Active Directory](authorize-access-azure-active-directory.md), [Autorización del acceso a recursos de Event Hubs mediante firmas de acceso compartido](authorize-access-shared-access-signature.md) |
| Authorization|  Sí | | [Autenticación de una identidad administrada con Azure Active Directory para acceder a recursos de Event Hubs](authenticate-managed-identity.md), [Autenticación de una aplicación con Azure Active Directory para acceder a recursos de Event Hubs](authenticate-application.md), [Autenticación del acceso a recursos de Event Hubs mediante firmas de acceso compartido (SAS)](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  Sí | |  |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sin |  |  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | |  |
| Llamadas a API cifradas| Sí |  |  |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |  |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).
