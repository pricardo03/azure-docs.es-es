---
title: Eliminación del grupo de recursos y de los recursos en Azure Resource Manager
description: Describe cómo Azure Resource Manager ordena la eliminación de recursos al eliminar un grupo de recursos. Describe los códigos de respuesta y cómo Resource Manager los controla para determinar si la eliminación se realizó correctamente.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2018
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: c38b1ccf7f7ccfe57e2b29f236f642238c4706a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61363675"
---
# <a name="azure-resource-manager-resource-group-deletion"></a>Eliminación del grupo de recursos en Azure Resource Manager

Este artículo describe cómo Azure Resource Manager ordena la eliminación de recursos al eliminar un grupo de recursos.

## <a name="determine-order-of-deletion"></a>Determinación del orden de eliminación

Cuando se elimina un grupo de recursos, Resource Manager determina el orden para eliminar los recursos. Usa el orden siguiente:

1. Se eliminan todos los recursos secundarios (anidados).

2. Los recursos que administran otros recursos se eliminan a continuación. Un recurso puede tener establecida la propiedad `managedBy` para indicar que un recurso diferente lo administra. Cuando se establece esta propiedad, se elimina el recurso que administra el otro recurso antes que los demás.

3. El resto de los recursos se elimina después de las dos categorías anteriores.

## <a name="resource-deletion"></a>Eliminación de recursos

Después de determinar el orden, el Administrador de recursos emite una operación de eliminación para cada recurso. Espera a que todas las dependencias finalicen antes de continuar.

En las operaciones sincrónicas, los códigos de respuesta correcta esperados son:

* 200
* 204
* 404

En las operaciones asincrónicas, la respuesta correcta esperada es 202. Resource Manager realiza el seguimiento del encabezado de ubicación o del encabezado de la operación asincrónica de Azure para determinar el estado de la operación de eliminación asincrónica.
  
### <a name="errors"></a>Errors

Cuando una operación de eliminación devuelve un error, Resource Manager vuelve a intentar la llamada a DELETE. Los reintentos se producen para los códigos de estado 5xx, 429 y 408. De forma predeterminada, el período de tiempo de reintento es de 15 minutos.

## <a name="after-deletion"></a>Después de la eliminación

Resource Manager emite una llamada GET en cada recurso que ha intentado eliminar. Se espera que la respuesta de la llamada sea 404. Cuando Resource Manager obtiene un error 404, considera que la eliminación se ha completado correctamente. Resource Manager quita el recurso de su memoria caché.

Sin embargo, si la llamada a GET en el recurso devuelve una respuesta 200 o 201, Resource Manager vuelve a crear el recurso.

### <a name="errors"></a>Errors

Si la operación GET devuelve un error, Resource Manager vuelve a intentar la operación GET para el código de error siguiente:

* Menor que 100
* 408
* 429
* Mayor que 500

Con otros códigos de error, Resource Manager no puede eliminar el recurso.

## <a name="next-steps"></a>Pasos siguientes

* Para comprender los conceptos de Resource Manager, consulte [Información general sobre Azure Resource Manager](resource-group-overview.md).
* Para los comandos de eliminación, consulte [PowerShell](/powershell/module/az.resources/Remove-AzResourceGroup), [CLI de Azure](/cli/azure/group?view=azure-cli-latest#az-group-delete), y [API de REST](/rest/api/resources/resourcegroups/delete).
