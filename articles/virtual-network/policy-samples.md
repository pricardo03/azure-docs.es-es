---
title: Ejemplos de plantillas de directivas | Microsoft Docs
description: Ejemplos de plantillas de directivas de Azure para Virtual Network.
services: virtual-network
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 8228da778e6cf6a874868ef0206092913908352d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694288"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Plantillas de ejemplo de directivas de Azure para Virtual Network.

En la siguiente tabla se incluyen vínculos a ejemplos de [Azure Policy](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Estos ejemplos se encuentran en el [repositorio de ejemplos de Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Red**||
| [NSG X en cada NIC](../governance/policy/samples/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere que se use un grupo de seguridad de red específico con cada interfaz de red virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [NSG X en cada subred](../governance/policy/samples/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere que se use un grupo de seguridad de red específico con cada subred virtual. Se especifica el identificador del grupo de seguridad de red que se va a usar. |
| [No hay tabla de rutas](../governance/policy/samples/no-user-defined-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Prohíbe que se implementen redes virtuales con una tabla de rutas. |
| [Usar subred aprobada para las interfaces de red de máquinas virtuales](../governance/policy/samples/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere que las interfaces de red usen una subred aprobada. Se especifica el identificador de la subred aprobada. |
| [Usar red virtual aprobada para las interfaces de red de máquinas virtuales](../governance/policy/samples/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere que las interfaces de red usen una red virtual aprobada. Se especifica el identificador de la red virtual aprobada. |
|**Supervisión**||
| [Auditar la configuración de diagnóstico](../governance/policy/samples/audit-diagnostic-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Audita si la configuración de diagnóstico no está habilitada para tipos de recursos especificados. Se especifica una matriz de tipos de recursos para comprobar si la configuración de diagnóstico está habilitada. |
|**Convenciones de nombre y texto**||
| [Permitir varios patrones de nombre](../governance/policy/samples/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Permite utilizar uno de varios patrones de nombre para los recursos. |
| [Requerir patrón Like](../governance/policy/samples/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Asegúrese de que los nombres de recursos cumplan la condición *like* para un patrón. |
| [Requerir patrón de coincidencia](../governance/policy/samples/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Asegúrese de que los nombres de recursos coincidan con un patrón de nomenclatura específico. |
| [Requerir patrón de coincidencia de etiqueta](../governance/policy/samples/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Asegúrese de que un valor de etiqueta coincida con un patrón de texto. |
|**Etiquetas**||
| [Iniciativa de directiva de etiquetas de facturación](../governance/policy/samples/billing-tags-policy-initiative.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere valores de etiqueta especificados para el nombre de producto y el centro de costos. Usa directivas integradas para aplicar las etiquetas requeridas. Se especifican los valores requeridos para las etiquetas.  |
| [Aplicar etiqueta y su valor en grupos de recursos](../governance/policy/samples/enforce-tag-on-resource-groups.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere una etiqueta y un valor en un grupo de recursos. Se especifica el valor y el nombre de etiqueta que se requieren.  |
| [Aplicar una etiqueta y su valor](../governance/policy/samples/enforce-tag-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere un valor y nombre de etiqueta especificada. Se especifica el valor y el nombre de la etiqueta que se va a aplicar.  |
| [Aplicar etiqueta y su valor predeterminado](../governance/policy/samples/apply-tag-default-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Anexa un nombre y valor de etiqueta especificado, si no se proporciona esa etiqueta. Se especifica el valor y el nombre de la etiqueta que se va a aplicar.  |
|**General**||
| [Ubicaciones permitidas](../governance/policy/samples/allowed-locations.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Requiere que todos los recursos se implementen en las ubicaciones aprobadas. Se especifica una matriz de ubicaciones aprobadas.  |
| [Tipos de recursos permitidos](../governance/policy/samples/allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Garantiza que solo se implementen los tipos de recursos aprobados. Se especifica una matriz de los tipos de recursos permitidos.  |
| [Tipos de recursos no permitidos](../governance/policy/samples/not-allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Prohíbe la implementación de tipos de recursos especificados. Se especifica una matriz de los tipos de recurso que se bloquearán.  |