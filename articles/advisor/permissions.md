---
title: Permisos en Azure Advisor
description: Los permisos del Asesor de actualizaciones y cómo se puede bloquear la capacidad de configurar las suscripciones o posponer o descartar recomendaciones.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: cbd2e456c96dbf8ca01387f0c7c17a1541dbfe55
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467619"
---
# <a name="permissions-in-azure-advisor"></a>Permisos en Azure Advisor

Azure Advisor proporciona recomendaciones basadas en el uso y la configuración de los recursos de Azure y las suscripciones. Usa el Asistente para la [roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) proporcionada por [Role-Based Access Control](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) para administrar el acceso a las recomendaciones y características del Asistente para. 

## <a name="roles-and-their-access"></a>Roles y su acceso

La siguiente tabla define los roles y el acceso que tienen dentro de Advisor:

| **Rol** | **Ver recomendaciones** | **Editar reglas** | **Editar configuración de la suscripción** | **Editar la configuración del grupo de recursos**| **Descartar y posponer las recomendaciones**|
|---|:---:|:---:|:---:|:---:|:---:|
|Propietario de la suscripción|**X**|**X**|**X**|**X**|**X**|
|Colaborador de la suscripción|**X**|**X**|**X**|**X**|**X**|
|Lector de la suscripción|**X**|--|--|--|--|
|Propietario del grupo de recursos|**X**|--|--|**X**|**X**|
|Colaborador del grupo de recursos|**X**|--|--|**X**|**X**|
|Lector de grupo de recursos|**X**|--|--|--|--|
|Propietario del recurso|**X**|--|--|--|**X**|
|Colaborador de recursos|**X**|--|--|--|**X**|
|Lector de recursos|**X**|--|--|--|--|

> [!NOTE]
> Acceso para ver recomendaciones es dependiente de los derechos de acceso a los recursos afectados de la recomendación.

## <a name="permissions-and-unavailable-actions"></a>Permisos y acciones disponibles

Falta de los permisos adecuados puede bloquear su capacidad para realizar acciones en Advisor. Estos son algunos problemas comunes.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>No se puede configurar suscripciones o grupos de recursos

Al intentar configurar suscripciones o grupos de recursos de Advisor, puede ver que la opción para incluir o excluir está deshabilitada. Este estado indica que no tiene un nivel suficiente de permisos para ese grupo de recursos o suscripción. Para resolver este problema, obtenga información sobre cómo [conceder acceso de usuario](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>No se puede posponer o descartar una recomendación

Si recibe un error al intentar posponer o descartar una recomendación, es posible que no tiene permisos suficientes. Asegúrese de que tiene al menos acceso de colaborador a los recursos afectados de la recomendación se posponer o descartar. Para resolver este problema, obtenga información sobre cómo [conceder acceso de usuario](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

## <a name="next-steps"></a>Pasos siguientes

Este artículo le da una visión general de cómo resolver problemas comunes y cómo Advisor usa RBAC para controlar los permisos de usuario. Para más información sobre Advisor, consulte:

- [¿Qué es Azure Advisor?](https://docs.microsoft.com/azure/advisor/advisor-overview)
- [Introducción a Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-get-started)
