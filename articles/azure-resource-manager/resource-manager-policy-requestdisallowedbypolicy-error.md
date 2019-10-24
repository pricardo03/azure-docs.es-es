---
title: Error RequestDisallowedByPolicy con la directiva de recursos de Azure | Microsoft Docs
description: Describe la causa del error RequestDisallowedByPolicy al implementar recursos con Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c791342bf68f84f6893e549d8528d1a861aa9040
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390301"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Error RequestDisallowedByPolicy con la directiva de recursos de Azure

Este artículo describe la causa del error RequestDisallowedByPolicy y también proporciona soluciones para este error.

## <a name="symptom"></a>Síntoma

Durante la implementación, es posible que reciba un error **RequestDisallowedByPolicy** que impida crear los recursos. El siguiente ejemplo muestra el error:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>solución de problemas

Para recuperar los detalles de la directiva que bloqueó la implementación, use uno de los métodos siguientes:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

En PowerShell, proporcione ese identificador de directiva como el parámetro `Id` para recuperar los detalles de la directiva que bloqueó la implementación.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>CLI de Azure

En la CLI de Azure, proporcione el nombre de la definición de directiva:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Solución

Por motivos de seguridad o conformidad, los administradores de la suscripción deben asignar directivas que limiten la forma en que se implementan los recursos. Por ejemplo, la suscripción puede tener una directiva que impide crear direcciones IP públicas, grupos de seguridad de red, rutas definidas por el usuario o tablas de rutas. En el mensaje de error de la sección **Síntoma** aparece el nombre de la directiva.
Para resolver este problema, revise las directivas de recursos y determine cómo implementar los recursos que cumplen con dichas directivas.

Para más información, consulte los siguientes artículos.

- [¿Qué es Azure Policy?](../governance/policy/overview.md)
- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)
