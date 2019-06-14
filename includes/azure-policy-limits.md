---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/05/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: ea87e51e66985f860cd6d10595a32facde1dc639
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67132952"
---
Hay un número máximo de cada tipo de objeto de Azure Policy. Una entrada de _Scope_ significa la suscripción o el [grupo de administración](../articles/governance/management-groups/overview.md).

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 500 |
| Ámbito | Definiciones de iniciativa | 100 |
| Inquilino | Definiciones de iniciativa | 1000 |
| Ámbito | Asignaciones de iniciativas o directivas | 100 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 100 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de iniciativas o directivas | Exclusiones (notScopes) | 400 |
| Regla de directiva | Condicionales anidados | 512 |
