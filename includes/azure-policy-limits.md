---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/16/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 0e5d1214a8e1af8299cb40d1a3b31ff6eafc8c5c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40184986"
---
Hay un número máximo de cada tipo de objeto de Azure Policy. Una entrada de _Scope_ significa la suscripción o el [grupo de administración](../articles/azure-resource-manager/management-groups-overview.md).

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 250 |
| Ámbito | Definiciones de iniciativa | 100 |
| Inquilino | Definiciones de iniciativa | 1000 |
| Ámbito | Asignaciones de iniciativa/directivas | 100 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 100 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de iniciativa/directivas | Exclusiones (notScopes) | 100 |
| Regla de directiva | Condicionales anidados | 512 |
