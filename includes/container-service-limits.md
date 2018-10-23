---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4251f379c517d5ccfd0430987e3d5280208590ff
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400193"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Máximo de clústeres por suscripción | 100 |
| Máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo: [red básica][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [red avanzada ][advanced-networking] con Azure CNI | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup> Cuando implementa un clúster de AKS con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable en hasta **110 pods por nodo**. No puede configurar un máximo de pods por nodo después de haber implementado un clúster AKS, o si implementa un clúster mediante Azure Portal.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
