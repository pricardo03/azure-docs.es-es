---
title: Información general de Azure Resource Graph
description: Sepa en modo en que el servicio Azure Resource Graph permite realizar consultas complejas de recursos a escala.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/29/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 28efdabc024fd32c83ba966b15284ec6ff368d4d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59269297"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Introducción al servicio Azure Resource Graph

Azure Resource Graph es un servicio de Azure diseñado para extender Azure Resource Management; para ello, proporciona una exploración de recursos eficaz y de alto rendimiento con la capacidad de consultar a escala a través de todas las suscripciones y grupos de administración para que pueda controlar eficazmente el entorno. Estas consultas proporcionan las siguientes características:

- Capacidad para consultar recursos con propiedades complejas de filtrado, agrupación y ordenación por recursos.
- Capacidad de explorar recursos según los requisitos de gobernanza de forma iterativa y convertir la expresión resultante en una definición de directiva.
- Capacidad de evaluar el impacto de aplicar directivas en un entorno de nube de gran capacidad.

En esta documentación, repasará cada característica en detalle.

> [!NOTE]
> Azure Resource Graph se usa en la nueva experiencia de exploración de "Todos los recursos" de Azure Portal. Se ha diseñado para ayudar a los clientes que necesitan administrar entornos a gran escala.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Cómo Resource Graph complementa Azure Resource Manager

Actualmente, Azure Resource Manager envía datos a una caché de recursos limitados que hace disponibles varios campos de recursos, específicamente: nombre del recurso, identificador, tipo, grupo de recursos, suscripciones y ubicación. Anteriormente, el trabajo con varias propiedades de recursos requería llamadas a cada proveedor de recursos individual y solicitar los detalles de las propiedades de cada recurso.

Con Azure Resource Graph, puede tener acceso a estas propiedades que devuelven los proveedores de recursos sin necesidad de realizar llamadas individuales a cada proveedor de recursos. Para obtener una lista de tipos de recurso compatibles, busque **Sí** en la tabla [Resources for complete mode deployments](../../azure-resource-manager/complete-mode-deletion.md) (Recursos para implementaciones en modo completo).

## <a name="the-query-language"></a>El lenguaje de consulta

Ahora que comprende mejor qué es Azure Resource Graph, vamos a profundizar en cómo crear consultas.

Es importante entender que el lenguaje de consulta de Azure Resource Graph se basa en el [lenguaje de consulta de Kusto](../../data-explorer/data-explorer-overview.md) que usa Azure Data Explorer.

En primer lugar, para obtener información sobre las operaciones y funciones que se pueden usar con Azure Resource Graph, consulte [Lenguaje de consulta de Resource Graph](./concepts/query-language.md). Para examinar los recursos, consulte [Explorar recursos](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Permisos en Azure Resource Graph

Para usar Resource Graph, debe tener los derechos adecuados en el [Control de acceso basado en rol](../../role-based-access-control/overview.md) (RBAC) con al menos acceso de lectura a los recursos que quiera consultar. Sin tener al menos permisos del tipo `read` para el objeto de Azure o el grupo de objetos, no se devolverán los resultados.

> [!NOTE]
> Resource Graph usa las suscripciones disponibles de una entidad de seguridad durante el inicio de sesión. Para ver los recursos de una nueva suscripción que se agregan durante una sesión activa, la entidad de seguridad debe actualizar el contexto. Esta acción se produce automáticamente al cerrar sesión y volver a iniciarla posteriormente.

## <a name="throttling"></a>Limitaciones

Las consultas a Resource Graph se limitan para proporcionar la mejor experiencia y tiempo de respuesta para todos los clientes. Si la organización desea utilizar Resource Graph API para consultas frecuentes y a gran escala, utilice el portal de comentarios de la página de Resource Graph. Asegúrese de proporcionar su caso de negocio y seleccione la casilla "Microsoft puede ponerse en contacto con usted por correo electrónico en relación con sus comentarios" para que el equipo pueda ponerse en contacto con usted.

## <a name="running-your-first-query"></a>Ejecución de la primera consulta

Resource Graph admite la CLI de Azure, Azure PowerShell, y el SDK para .NET de Azure. La consulta se estructura igual para cada lenguaje. Obtenga información sobre cómo habilitar Resource Graph en la [CLI de Azure](first-query-azurecli.md#add-the-resource-graph-extension) y [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Pasos siguientes

- Ejecución de la primera consulta con la [CLI de Azure](first-query-azurecli.md)
- Ejecución de la primera consulta con [Azure PowerShell](first-query-powershell.md)
- Comience con [consultas de inicio](./samples/starter.md)
- Mejore sus conocimientos con [consultas avanzadas](./samples/advanced.md)