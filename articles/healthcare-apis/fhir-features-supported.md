---
title: 'Características de FHIR admitidas en Azure: Azure API for FHIR'
description: En este artículo se explica qué características de la especificación de FHIR se implementan en Azure API for FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 74a17c4a433ebe1c1107230739086375fe165ed9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033774"
---
# <a name="features"></a>Características

Azure API for FHIR proporciona una implementación totalmente administrada Microsoft FHIR Server para Azure. El servidor es una implementación del estándar [FHIR](https://hl7.org/fhir). En este documento se enumeran las características principales de FHIR Server.

## <a name="fhir-version"></a>Versión de FHIR

La versión más reciente compatible (disponible solo en la implementación de software de código abierto): `4.0.0`

Las versiones anteriores también admitidas actualmente incluyen (disponibles en las implementaciones PaaS y OSS): `3.0.1`

## <a name="rest-api"></a>API DE REST

| API                            | Compatible | Comentario |
|--------------------------------|-----------|---------|
| read                           | Sí       |         |
| vread                          | Sí       |         |
| update                         | Sí       |         |
| update with optimistic locking | Sí       |         |
| update (conditional)           | Sin        |         |
| patch                          | Sin        |         |
| delete                         | Sí       |         |
| delete (conditional)           | Sin        |         |
| create                         | Sí       | Admite POST y PUT |
| create (conditional)           | Sin        |         |
| búsqueda                         | Parcial   | Consulte a continuación |
| capabilities                   | Sí       |         |
| proceso por lotes                          | Sin        |         |
| transaction                    | Sin        |         |
| history                        | Sí       |         |
| paging                         | Parcial   | `self` y `next` se admiten |
| intermediaries                 | Sin        |         |

## <a name="search"></a>Search

Se admiten todos los tipos de parámetro de búsqueda. *No* se admiten los parámetros encadenados y el encadenamiento inverso.

| Tipo de parámetro de búsqueda | Compatible | Comentario |
|-----------------------|-----------|---------|
| Number                | Sí       |         |
| Date/DateTime         | Sí       |         |
| Cadena                | Sí       |         |
| Se necesita el cifrado de tokens                 | Sí       |         |
| Referencia             | Sí       |         |
| Compuesto             | Sí       |         |
| Cantidad              | Sí       | Problema [#103](https://github.com/Microsoft/fhir-server/issues/103) |
| URI                   | Sí       |         |


| Modificadores             | Compatible | Comentario |
|-----------------------|-----------|---------|
|`:missing`             | Sí       |         |
|`:exact`               | Sí       |         |
|`:contains`            | Sí       |         |
|`:text`                | Sí       |         |
|`:in` (token)          | Sin        |         |
|`:below` (token)       | Sin        |         |
|`:above` (token)       | Sin        |         |
|`:not-in` (token)      | Sin        |         |
|`:[type]` (referencia)  | Sin        |         |
|`:below` (uri)         | Sí       |         |
|`:above` (uri)         | Sin        | Problema [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Parámetro de búsqueda común | Compatible | Comentario |
|-------------------------| ----------|---------|
| `_id`                   | Sí       |         |
| `_lastUpdated`          | Sí       |         |
| `_tag`                  | Sí       |         |
| `_profile`              | Sí       |         |
| `_security`             | Sí       |         |
| `_text`                 | No        |         |
| `_content`              | No        |         |
| `_list`                 | No        |         |
| `_has`                  | No        |         |
| `_type`                 | Sí       |         |
| `_query`                | Sin        |         |

| Operaciones de búsqueda       | Compatible | Comentario |
|-------------------------|-----------|---------|
| `_filter`               | Sin        |         |
| `_sort`                 | No        |         |
| `_score`                | No        |         |
| `_count`                | Sí       |         |
| `_summary`              | Parcial   | `_summary=count` se admite |
| `_include`              | Sin        |         |
| `_revinclude`           | No        |         |
| `_contained`            | No        |         |
| `_elements`             | Sin        |         |

## <a name="persistence"></a>Persistencia

Microsoft FHIR Server tiene un módulo de persistencia conectable (consulte [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Actualmente, el código fuente abierto de FHIR Server incluye una implementación para [Azure Cosmos DB](../cosmos-db/index-overview.md).

Cosmos DB es una base de datos (SQL API, MongoDB API, etc.) de varios modelos distribuida de forma global. Admite diferentes [niveles de coherencia](../cosmos-db/consistency-levels.md). La plantilla de implementación predeterminada configura FHIR Server con una coherencia de `Strong`, pero la directiva de coherencia puede modificarse (generalmente de forma más relajada) en función de cada solicitud mediante el encabezado de solicitud `x-ms-consistency-level`.

## <a name="role-based-access-control"></a>Control de acceso basado en rol

FHIR Server utiliza [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) para el control de acceso. En concreto, se aplica Control de acceso basado en rol (RBAC), si el parámetro de configuración `FhirServer:Security:Enabled` se establece en `true` y todas las solicitudes (excepto `/metadata`) a FHIR Server deben tener el encabezado de solicitud `Authorization` establecido en `Bearer <TOKEN>`. El token debe contener uno o varios roles, tal como se define en la notificación `roles`. Se permitirá una solicitud si el token contiene un rol que permite la acción especificada en el recurso especificado.

Actualmente, las acciones permitidas para un rol determinado se aplican *globalmente* en la API.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha leído sobre las características admitidas de FHIR en Azure API for FHIR. A continuación, implemente una API de FHIR en Azure.
 
>[!div class="nextstepaction"]
>[Implementación de FHIR Server de código abierto](fhir-oss-powershell-quickstart.md)