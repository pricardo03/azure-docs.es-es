---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 71294824bd3dd5215c388cfcd44382c7eee123ad
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48874077"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Máximo de clústeres por suscripción | 100 |
| Máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo: [red básica][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [red avanzada ][advanced-networking] con Azure CNI | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup> Este valor es configurable en la implementación de clústeres al implementar un clúster de AKS con la CLI de Azure o una plantilla de Resource Manager.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
