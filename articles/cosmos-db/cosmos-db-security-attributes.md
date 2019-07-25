---
title: Atributos de seguridad de Azure Cosmos DB
description: Lista de comprobación de los atributos de seguridad para evaluar Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: ffeb60f5476a540e3da46a82c240b0dda9aa6be2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480469"
---
# <a name="security-attributes-for-azure-cosmos-db"></a>Atributos de seguridad de Azure Cosmos DB

En este artículo se documentan los atributos de seguridad comunes integrados en Azure Cosmos DB.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Todas las bases de datos de Cosmos DB y las copias de seguridad se cifran de forma predeterminada; consulte [Cifrado de datos en Azure Cosmos DB](database-encryption-at-rest.md). No se admite el cifrado del servidor con claves administradas por el cliente. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en red virtual y cifrado de red virtual a red virtual)| Sí | Todos los datos de Azure Cosmos DB se cifran en tránsito. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| Sí | Solo en las instancias de Table API Premium. No todas las API admiten esta característica. Consulte [Introducción a Azure Cosmos DB: Table API](table-introduction.md). |
| Llamadas a API cifradas| Sí | Todas las conexiones a Azure Cosmos DB admiten HTTPS. Azure Cosmos DB también admite las conexiones TLS 1.2, pero esto no es obligatorio todavía. Si los clientes desactivan la conexión TLS de nivel inferior por su parte, pueden garantizar la conexión a Cosmos DB.  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de red virtual| Sí | Con el punto de conexión de servicio de red virtual, puede configurar la cuenta de Azure Cosmos DB para permitir el acceso solo desde una subred específica de una red virtual (VNet). También puede combinar el acceso de red virtual con reglas de firewall.  Consulte [Acceso a Azure Cosmos DB desde redes virtuales](vnet-service-endpoint.md) |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Gracias a la compatibilidad con el firewall, puede configurar su cuenta de Azure Cosmos para permitir el acceso solo desde un conjunto de direcciones IP aprobado, un intervalo de direcciones IP o servicios en la nube. Consulte [Configuración del firewall de IP en Azure Cosmos DB](how-to-configure-firewall.md).|
| Compatibilidad con tunelización forzada | Sí | Se puede configurar en el cliente en la red virtual donde se encuentran las máquinas virtuales.   |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Se registran todas las solicitudes que se envían a Azure Cosmos DB. Se admiten la [supervisión de Azure](../azure-monitor/overview.md), las métricas de Azure y el registro de auditoría de Azure.  Puede registrar la información correspondiente a las solicitudes de plano de datos, las estadísticas de tiempo de ejecución de consultas, el texto de la consulta y las solicitudes de MongoDB. También puede configurar alertas. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Authentication| Sí | Sí en el nivel de cuenta de base de datos; en el nivel del plano de datos, Cosmos DB usa tokens de recursos y acceso con clave. |
| Autorización| Sí | Se admite en la cuenta de Azure Cosmos con las claves maestras (principal y secundaria) y tokens de recursos. Puede obtener acceso de lectura/escritura o acceso de solo lectura a los datos con las claves maestras. Los tokens de recursos permiten el acceso de tiempo limitado a los recursos, como documentos y contenedores. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | El registro de actividad de Azure para las operaciones de nivel de cuenta, como Firewalls, redes virtuales, acceso con claves e IAM. |
| Registro y auditoría del plano de datos | Sí | Registro de supervisión de diagnósticos para las operaciones de nivel de contenedor, como la creación contenedores, el aprovisionamiento de rendimiento, la indexación de directivas y las operaciones CRUD en documentos. |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sin  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Atributos de seguridad adicionales para Cosmos DB

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Uso compartido de recursos entre orígenes (CORS) | Sí | Consulte [Configuración del uso compartido de recursos entre orígenes (CORS)](how-to-configure-cross-origin-resource-sharing.md). |
