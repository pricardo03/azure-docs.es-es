---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: mmacy
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 1e8913d31677f3da9b6eb9d2216d8d9ec8b186b4
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667036"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Máximo de clústeres por suscripción | 100 |
| Máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo: [red básica][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [red avanzada ][advanced-networking] con Azure CNI | Implementación de la CLI de Azure: 110<sup>1</sup><br />Plantilla de Resource Manager: 110<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup> Este valor es configurable en la implementación de clústeres al implementar un clúster de AKS con la CLI de Azure o una plantilla de Resource Manager.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
