---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 57cec39bde460c6079091490acf541761c61e003
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554052"
---
Hay un número máximo para cada tipo de objeto de directiva de Azure. Una entrada de _Scope_ significa la suscripción o el [grupo de administración](../articles/governance/management-groups/overview.md).

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 250 |
| Ámbito | Definiciones de iniciativa | 100 |
| Inquilino | Definiciones de iniciativa | 1000 |
| Ámbito | Asignaciones de directiva o iniciativa | 100 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 100 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de directiva o iniciativa | Exclusiones (notScopes) | 250 |
| Regla de directiva | Secciones condicionales anidadas | 512 |
