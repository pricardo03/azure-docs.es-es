---
title: Atributos de seguridad para Azure Cosmos DB
description: Una lista de comprobación de los atributos de seguridad para evaluar Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: 891a8389d7e34c48a00dda8fdebd251a698e5937
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420654"
---
# <a name="security-attributes-azure-cosmos-db"></a>Atributos de seguridad de Azure Cosmos DB

Este artículo documenta los atributos comunes de seguridad integrados en Azure Cosmos DB.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Todas las bases de datos de Cosmos DB y las copias de seguridad se cifran de forma predeterminada; consulte [cifrado de datos en Azure Cosmos DB](database-encryption-at-rest.md). No se admite el cifrado del lado servidor con claves administradas por el cliente. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y redes)| Sí | Todos los datos de Azure Cosmos DB se cifran en tránsito. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| Sí | Solo en el Premium de API de tablas. No todas las API admiten esta característica. Consulte [Introducción a Azure Cosmos DB: API de tabla](table-introduction.md). |
| Llamadas a API cifradas| Sí | Todas las conexiones a Azure Cosmos DB admiten HTTPS. Azure Cosmos DB también admite las conexiones TLS 1.2, pero esto no es obligatorio todavía. Si los clientes desactivar TLS de nivel inferior en su extremo, pueden asegurarse de para conectarse a Cosmos DB.  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de red virtual| Sí | Con el punto de conexión de servicio de red virtual, puede configurar una cuenta de Azure Cosmos DB para permitir el acceso solo desde una subred específica de una red virtual (VNet). También puede combinar el acceso de red virtual con reglas de firewall.  Consulte [acceso a Azure Cosmos DB desde redes virtuales](vnet-service-endpoint.md). |
| Compatibilidad con aislamiento de red y Firewalling| Sí | Con compatibilidad con el firewall, puede configurar su cuenta de Azure Cosmos para permitir el acceso solo desde un conjunto de direcciones IP, un intervalo de direcciones IP aprobado o servicios en la nube. Consulte [firewall para configurar direcciones IP en Azure Cosmos DB](how-to-configure-firewall.md).|
| Compatibilidad con tunelización forzada | Sí | Se puede configurar en el lado del cliente en la red virtual donde se encuentran las máquinas virtuales.   |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Se registran todas las solicitudes que se envían a Azure Cosmos DB. [Supervisión de Azure](../azure-monitor/overview.md), se admiten las métricas de Azure, el registro de auditoría de Azure.  Puede registrar la información correspondiente a las solicitudes de plano de datos, las estadísticas de tiempo de ejecución de consultas, texto de la consulta, solicitudes de MongoDB. También puede configurar alertas. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Authentication| Sí | Sí en el nivel de cuenta de base de datos; en el nivel del plano de datos, Cosmos DB usa tokens de recursos y acceso a la clave. |
| Autorización| Sí | Se admite en la cuenta de Azure Cosmos con las claves maestras (principal y secundaria) y los tokens de recursos. Puede obtener de lectura/escritura o solo acceso a datos con las claves maestras de lectura. Los tokens de recursos permiten el acceso de tiempo limitado a los recursos como documentos y contenedores. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | Registro de actividad de Azure para las operaciones de nivel de cuenta, como Firewalls, redes virtuales, claves de acceso y IAM. |
| Registro y auditoría del plano de datos | Sí | Diagnósticos de supervisión de registro para las operaciones de nivel de contenedor, como creación contenedor de aprovisionamiento del rendimiento, indexación de las directivas y las operaciones CRUD en documentos. |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sin  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Atributos de seguridad adicionales para Cosmos DB

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Cross Origin Resource Sharing, (CORS) | Sí | Consulte [configurar recursos entre orígenes (CORS) de uso compartido](how-to-configure-cross-origin-resource-sharing.md). |
