---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/30/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 7907504401f4b47aafe6032ea895d9647e6c303c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420788"
---
Hay un número máximo de cada tipo de objeto de Azure Policy. Una entrada de _Scope_ significa la suscripción o el [grupo de administración](../articles/governance/management-groups/overview.md).

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 250 |
| Ámbito | Definiciones de iniciativa | 100 |
| Inquilino | Definiciones de iniciativa | 1000 |
| Ámbito | Asignaciones de iniciativas o directivas | 100 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 100 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de iniciativas o directivas | Exclusiones (notScopes) | 400 |
| Regla de directiva | Condicionales anidados | 512 |
