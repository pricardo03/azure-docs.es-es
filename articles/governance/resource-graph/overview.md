---
title: Información general de Azure Resource Graph
description: Azure Resource Graph es un servicio de Azure que permite realizar consultas complejas de recursos a escala.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d68183f4d0a928ac72f3f73ea5225ad174820cb7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162107"
---
# <a name="what-is-azure-resource-graph"></a>Qué es Azure Resource Graph

Azure Resource Graph es un servicio de Azure diseñado para extender Azure Resource Management; para ello, proporciona una exploración de recursos eficaz y de alto rendimiento con la capacidad de consultar a escala a través de todas las suscripciones y grupos de administración para que pueda controlar eficazmente el entorno. Estas consultas proporcionan las siguientes capacidades:

- Capacidad para consultar recursos con propiedades complejas de filtrado, agrupación y ordenación por recursos.
- Capacidad de explorar recursos según los requisitos de regulación de forma iterativa y convertir la expresión resultante en una definición de directiva.
- Capacidad de evaluar el impacto de aplicar directivas en un entorno de nube de gran capacidad.

En esta documentación, repasará cada capacidad en detalle.

> [!NOTE]
> Azure Resource Graph se usa en la nueva experiencia de exploración de "Todos los recursos" de Azure Portal. Se ha diseñado para ayudar a los clientes que necesitan administrar entornos a gran escala.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Cómo Resource Graph complementa Azure Resource Manager

Actualmente, Azure Resource Manager envía datos a una caché de recursos limitados que expone varios campos de recursos, específicamente: nombre del recurso, identificador, tipo, grupo de recursos, suscripciones y ubicación. En la actualidad, si deseara trabajar con más propiedades de recursos, tendría que realizar llamadas a cada proveedor de recursos individual y solicitar los detalles de las propiedades de cada recurso.

Con Azure Resource Graph, puede tener acceso a estas propiedades que devuelven los proveedores de recursos sin necesidad de realizar llamadas individuales a cada proveedor de recursos.

## <a name="the-query-language"></a>El lenguaje de consulta

Ahora que comprende mejor qué es Azure Resource Graph, vamos a profundizar en cómo crear consultas.

Es importante entender que el lenguaje de consulta de Azure Resource Graph se basa en el [lenguaje de consulta de Azure Data Explorer](../../data-explorer/data-explorer-overview.md).

En primer lugar, para obtener información sobre las operaciones y funciones que se pueden usar con Azure Resource Graph, consulte [Lenguaje de consulta de Resource Graph](./concepts/query-language.md). Para examinar los recursos, consulte [Explorar recursos](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Permisos en Azure Resource Graph

Para usar Resource Graph, debe estar autorizado mediante el [Control de acceso basado en rol](../../role-based-access-control/overview.md) (RBAC) con al menos acceso de lectura a los recursos que quiera consultar. Si no tiene permisos `read` en el grupo de administración, suscripción, grupo de recursos o recurso individual, no se devolverá en los resultados de una consulta de Resource Graph.

## <a name="running-your-first-query"></a>Ejecución de la primera consulta

Resource Graph es compatible con la CLI de Azure y Azure PowerShell. El componente de consulta está estructurado de la misma manera con independencia de qué idioma se utilice. Azure Resource Graph todavía no es compatible de forma predeterminada con cualquier SDK, por lo que se debe cargar una extensión o un módulo para proporcionar los comandos necesarios.
Obtenga información sobre cómo habilitar Resource Graph en la [CLI de Azure](first-query-azurecli.md#add-the-resource-graph-extension) y [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Pasos siguientes

- Ejecución de la primera consulta con la [CLI de Azure](first-query-azurecli.md)
- Ejecución de la primera consulta con [Azure PowerShell](first-query-powershell.md)
- Comience con [consultas de inicio](./samples/starter.md)
- Mejore sus conocimientos con [consultas avanzadas](./samples/advanced.md)